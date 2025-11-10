# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

**MSARE (مساري)** is an AR-powered smart stadium navigation system built with Flutter that guides fans from parking to their seats and back using indoor positioning and augmented reality. The project aligns with Saudi Vision 2030 and targets real stadium deployments.

## Tech Stack

- **Frontend**: Flutter (Dart) for cross-platform mobile (iOS & Android)
- **Backend**: Firebase (Cloud Firestore, Auth, Storage)
- **Indoor Positioning**: Situm SDK for venue mapping and wayfinding
- **AR Framework**: ARCore (Android) / ARKit (iOS)
- **Cloud Functions**: Node.js (planned for IoT camera/AI crowd analytics integration)
- **UI Export**: Builder.io Visual Copilot for Figma → Flutter conversion
- **Design**: Figma for UI/UX design and prototyping

## System Architecture

### Core User Flow
1. **Authentication** → Login with Google or OTP
2. **Ticket Verification** → Validate ticket via QR/API
3. **Smart Parking Suggestion** → AI-based parking recommendation near seat
4. **Vehicle Navigation** → Outdoor/indoor driving directions to parking spot
5. **Bookmark Parking** → Save parking location for return journey
6. **Fan Navigation** → Walking directions from parking to seat with AR overlays
7. **Exit Guidance** → Recommend optimal exit gates based on real-time crowd density

### Navigation System (Dual Mode)
- **Vehicle Navigation**: Guides cars to parking spots (outdoor → indoor transition)
- **Fan Navigation**: Guides pedestrians from parking to seats (indoor AR-based)

Both modes support:
- Map-based navigation
- AR overlay navigation
- Optional stops/waypoints
- Real-time route recalculation
- Reverse navigation (return to parking)

### External Services Integration
- **Firebase**: Authentication, real-time database (parking/crowd data), storage
- **Situm SDK / Mappedin**: Indoor map routing API
- **AI CCTV System**: Live congestion data for crowd density visualization
- **MSARE Data Store**: Parking availability, reservation sync

### Key Design Constraints
- **Ticket-Parking Linking**: Handle edge cases (1 vehicle with 5 tickets, 50 users heading to same spot)
- **Real-time Sync**: Prevent "available" vs "reserved" conflicts
- **Sensor Integration**: Support for parking sensors and ANPR cameras at gates
- **Multi-age Accessibility**: Simple UI with icons over text, high contrast, large buttons

## Development Commands

### Flutter Project Setup (Once Implemented)
```pwsh
# Install dependencies
flutter pub get

# Run on connected device/emulator
flutter run

# Run on specific device
flutter devices
flutter run -d <device-id>

# Run a specific test file
flutter test test/path/to/test_file.dart

# Run all tests
flutter test

# Build for Android
flutter build apk --release

# Build for iOS (requires macOS)
flutter build ios --release

# Check for issues
flutter doctor

# Format code
dart format .

# Analyze code
flutter analyze
```

### Git Workflow
```pwsh
# Create feature branch
git checkout -b feature/your-feature-name

# Stage and commit changes
git add .
git commit -m "Add: Brief description"

# Push to fork
git push origin feature/your-feature-name

# Keep fork updated with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Commit Message Convention
- `Add: <description>` - New features
- `Fix: <description>` - Bug fixes
- `Update: <description>` - Documentation or configuration updates
- `Refactor: <description>` - Code refactoring

## Code Organization Principles

### UI Generation Strategy
- Use **Builder.io Visual Copilot** to export pixel-perfect Flutter widgets from Figma designs
- Keep UI components separate from business logic
- All Firebase, Situm, and AR integrations stay in native Flutter code (not visual builders)
- Target 80% of non-AR screens generated via export with ≤20% manual fixes

### Functional Requirements Structure
When implementing features, follow this hierarchy:

**FR-1: Smart Parking Suggestion**
- Seat-based parking recommendation
- AI crowd detection integration

**FR-2: Navigation**
- FR-2.1: Map Navigation
- FR-2.2: AR Navigation
- FR-2.3: Optional Stops
- FR-2.4: Route Recalculation
- FR-2.5: Reverse Navigation (Return to Parking)

**FR-3: Bookmark Parking Location**
- Save parking spot for return journey

**FR-4: Exit Gate Navigation**
- FR-4.1: Exit Gate Suggestion
- FR-4.2: Multi-Gate Selection
- FR-4.3: Traffic Visualization (Color Codes)
- FR-4.4: Real-Time Traffic Update

### Security & Authentication
- Authentication (Login/Sign-up) is a **non-functional requirement** (security concern)
- All tickets must be validated through a verification system
- Prevent fake/invalid tickets via QR codes or external API integration

## Critical Implementation Notes

### AR & Indoor Positioning
- AR calibration requires buffer time for anchoring, lighting adjustments, and route validation
- Test AR pose accuracy and drift thresholds at target venues
- Situm SDK handles indoor positioning; ARCore/ARKit renders visual overlays
- Keep AR logic fully customizable in native Flutter plugins

### Real-time Data Management
- Parking availability must sync with reservation status to prevent conflicts
- Crowd density data flows from AI CCTV system to app via Firebase/Firestore
- Handle concurrent users targeting same parking spot gracefully

### UI/UX Accessibility
- Design for all age groups: simple, icon-heavy, minimal text
- High contrast colors and large touch targets
- Short, clear instructions with visual aids (arrows, icons)

### Testing Strategy
- Test both vehicle and fan navigation paths separately
- Validate ticket verification edge cases
- Test AR alignment in various lighting conditions
- Verify real-time sync under load (multiple concurrent users)

## Design Assets

UI mockups are available in `Visily-Export_05-11-2025_01-18/`:
- Sign-in.png
- Ticket Scan.png
- Permissions.png
- Parking Suggestions.png
- Drive Navigation.png
- Save Parking.png
- Seat Input.png
- Indoor AR Navigation.png
- Arrived at Seat.png
- Return to Parking.png
- Recommended Entry Gate.png

## Documentation

- `README.md` - Project overview and contribution guidelines
- `docs/why-builder-io.md` - Rationale for Builder.io Visual Copilot choice
- `doctor-nots.md` - Academic supervisor feedback (in Arabic)
- `use case` - System flow diagram

## Future Integrations
- IoT parking sensors for real-time spot detection
- ANPR cameras at gates for automatic vehicle registration
- Node.js Cloud Functions for AI-powered crowd analytics
