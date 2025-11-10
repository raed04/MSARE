# Parking & Ticket Operations
This reference captures the high-level Firestore schema and verification flow that support the real-time parking reservations described in `WARP.md`. It also explains how the client listens to those documents so “Available” and “Reserved” never collide, even when many fans compete for the same spot.

## 1. Firestore schema sketch

### `parkingSlots/{slotId}`
| Field | Type | Purpose |
| --- | --- | --- |
| `zoneId` | string | The parking area (e.g., “Lot-A:Tier-2”). |
| `status` | string | Enum: `available`, `capture`, `reserved`, `occupied`, `blocked`. |
| `ticketId` | string \| null | Single ticket that currently owns the slot. |
| `ticketGroupId` | string \| null | Optional reference when a car has multiple linked tickets (5+ tickets per vehicle). |
| `vehicleId` | string \| null | Captured plate, device, or session ID for the connected car. |
| `captureLeaseExp` | timestamp | A short-lived (e.g., 30s) lease that prevents others from racing the slot; expires automatically through a background task or security rule. |
| `sensorState` | map | Latest IoT/camera reading (`presence`, `confidence`, `lastHeartbeat`). |
| `lastUpdated` | timestamp | Useful for UI staleness detection and logging. |
| `contenders` | array | Optional list of `waitlistEntryId` values—helps the UI show “50 fans in queue”. |

Indexes: composite over `zoneId` + `status` for efficient listing, and on `captureLeaseExp` for cleanup jobs.

### `ticketReservations/{reservationId}`
| Field | Type | Purpose |
| --- | --- | --- |
| `ticketId` | string | Paid ticket identifier (matches QR/API). |
| `ticketGroupId` | string \| null | Group ID for multi-ticket vehicles. |
| `userId` | string | Firebase Auth UID or guest token. |
| `slotId` | string | Reserved slot reference (if assigned). |
| `status` | string | `pending`, `reserved`, `arrived`, `cancelled`, `timeout`. |
| `arrivalWindow` | map | Planned arrival (`start`, `end`). |
| `verifiedAt` | timestamp | When the ticket was successfully verified. |
| `expiresAt` | timestamp | When the reservation or capture lease expires. |
| `source` | string | `qr`, `api`, `admin`, etc. |
| `notes` | map | Holds issues such as “camera mismatch”, “multi-ticket linked”. |

### `ticketGroups/{groupId}`
| Field | Type | Purpose |
| --- | --- | --- |
| `ownerId` | string | Matches `userId` for lead ticket. |
| `ticketIds` | array | All ticket IDs bound to the vehicle. |
| `vehicleId` | string | Shared vehicle identifier or plate. |
| `createdAt` | timestamp | For auditing. |
| `slotId` | string \| null | Slot claimed by the group. |

### `slotWaitlists/{slotId}/entries/{entryId}`
| Field | Type | Purpose |
| --- | --- | --- |
| `ticketReservationId` | string | Back-link to the waiting reservation. |
| `priority` | integer | Helps order crowds (arrival time or tier). |
| `requestedAt` | timestamp | When the waitlist request was created. |
| `status` | string | `queued`, `notified`, `abandoned`. |

Additional audit/log collections may capture camera events or ANPR readings so updates can later reconcile “slot looked empty but reservation is held”.

## 2. Ticket Verification & Capture function

### `verifyTicketReservation`
This Cloud Function (HTTP callable or REST webhook) is the gatekeeper invoked before writing to the schema above.

1. **Input**: `ticketPayload` (QR string or REST payload), `userId`, optionally `groupHint`, `preferredZone`.
2. **Validate** the ticket with the official API or trusted QR decoder.
   - Reject immediately if the ticket is expired, already redeemed, or flagged malicious.
   - For repeated tickets from the same vehicle, build/lookup a `ticketGroup`.
3. **Check current assignments** so a ticket can’t hold two slots; inspect `ticketReservations` and `parkingSlots`.
4. **Determine preferred slot** (AI suggestion) and start a Firestore transaction:
   - Lock the `parkingSlots/{slotId}` doc inside the transaction.
   - If `status == available`, move it to `capture` and set `captureLeaseExp = now + leaseDuration`.
   - Populate `ticketId`, `ticketGroupId`, `vehicleId`, `sensorState` overrides, `lastUpdated`.
   - Save/merge the reservation document (`status = reserved`, `slotId`, `expiresAt`, etc.).
5. **Release or escalate**: if the transaction fails because the slot is no longer available, add the reservation to the waitlist and return the next best zones.
6. **Return payload**: reserved `slotId`, updated lease timestamp, and `contenderCount`.
7. **Post-processing**: schedule TTL cleanup (via Cloud Task or scheduled function) that reverts `capture` to `available` if `captureLeaseExp` expires without `status` updates.

The function also enriches the WebUI: it can log `contenders` in the `parkingSlots` doc so that 50 people waiting for the same spot are visible to ops staff and the app.

## 3. Client-side sync & contention handling

1. **Listen to `parkingSlots` streams** filtered by zone: every fan’s map is backed by a Firestore listener to `status`, `captureLeaseExp`, and `contenders`.
2. **Resolve race states**: treat `capture` as “suggested but still committing”. Only switch UI to the reserved slot when the reservation document’s `status` flips to `reserved/arrived`.
3. **Watch waitlists**: for hotspots with dozens of queued users, surface a “Queue position #N” badge and suggest alternate slots once `slotWaitlists/{slotId}` emits a `notified` entry.
4. **Camera-sensor reconciliation**: rely on `sensorState` (ANPR and occupancy sensors) rather than a single camera frame. If the camera shows empty but the slot is `reserved`, the app displays the reservation as “confirmed” and may show a warning badge “sensors still match reservation”.
5. **Conflict recovery**: Firestore security rules should prevent clients from writing `status = reserved` directly; they must go through the verification function so double bookings and fake tickets are rejected before they reach the shared schema.

This setup keeps the “Available” vs “Reserved” narrative consistent with the real-time data requirements from `WARP.md:158-173`, even under heavy contests or multi-ticket vehicles.
