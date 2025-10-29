# Why We’re Choosing Builder.io Visual Copilot for MSARE

MSARE (مساري) targets a Flutter mobile app backed by Firebase, enriched with Situm indoor positioning and ARCore/ARKit navigation. We are standardizing on Builder.io’s Visual Copilot for Figma → Flutter export so designers can ship pixel-perfect UI fast, while engineers keep all AR, Situm, and Firebase integrations in native Flutter code.

## 1. One-Sentence Rationale
Builder.io gives us the fastest, most faithful path from Figma to clean Flutter widgets—perfect for shipping high-fidelity UI quickly while keeping full control over the AR/Situm integrations and Firebase logic in code.

## 2. What We Need (and What We Don’t)
- **We need**: Pixel-accurate Figma → Flutter export, responsive layout handling, readable code for developers, effortless handoff into our Flutter repo.
- **We don’t need**: A no-code backend builder or visual data layer; Firebase, Situm, and ARCore/ARKit already anchor our stack.

## 3. Why Builder.io Fits MSARE
### A. High-Fidelity Figma → Flutter
- Near pixel-perfect export of Figma frames into structured Flutter widget trees.
- Converts Auto Layout into responsive layouts, slashing manual rebuild time.

### B. Developer-First Handoff
- Generates standard Flutter widgets without proprietary runtimes or hidden dependencies.
- Developers can fold exports directly into our repo and continue with familiar state management, routing, and theming patterns.

### C. Clean Separation of Concerns
- Builder.io handles visual UI generation for onboarding, auth, parking list/map, live crowd dashboards, and profile/settings flows.
- Engineers wire Firebase, Situm, and AR through native Flutter packages, keeping critical positioning, sensor, and AR logic fully customizable.

### D. Faster Iterations From Design
- Designers iterate in Figma → export → developers connect data and logic.
- Reduces back-and-forth on pixel polish, freeing engineering cycles for AR alignment and route accuracy.

## 4. Why Not Make FlutterFlow the Primary Path?
- FlutterFlow excels for no-/low-code CRUD apps, but advanced AR + indoor positioning via custom plugins stays smoother in pure Flutter.
- We already own the backend with Firebase, so visual data bindings add little value and may obscure complex routing or sensor fusion logic.
- FlutterFlow remains optional for lightweight admin dashboards (e.g., parking capacity CMS) but not for the AR-centric core experience.

## 5. Comparison Focused on Our Needs
| Focus Area | Builder.io (Figma → Flutter) | FlutterFlow (Primary App Path) |
| --- | --- | --- |
| Figma → Flutter fidelity | Excellent: responsive, clean widgets | Good: newer import pipeline, more cleanup |
| Firebase wiring | Manual in code using standard patterns | Visual bindings available, but redundant for us |
| Situm + ARCore/ARKit | Best achieved in pure Flutter with plugins | Possible but harder to maintain in visual builder |
| Dev handoff | Direct: drop exports into repo | Export exists but adds visual-layer abstractions |
| Risk of lock-in | None: outputs plain Flutter code | Low but higher friction for complex plugins |
| Best MSARE use | Main Figma → Flutter UI path | Optional for ops/admin tooling only |

## 6. Expected Impact
- **Speed**: Cut build time for non-AR screens by 30–50%.
- **Quality**: Achieve high layout fidelity with fewer post-export tweaks.
- **Focus**: Redirect engineering effort to AR alignment, indoor routing, and real-time data integrations.

## 7. Implementation Plan (Week 1)
1. **Design (Figma)**: Finalize Auth, Home, Parking (list/map), Live Crowd, Route Preview, Settings, and Profile screens.
2. **Export**: Use Builder.io Visual Copilot to produce Flutter widgets for those screens.
3. **Project setup (Flutter)**: Integrate Firebase (Auth, Firestore, Storage), add Situm SDK for venue maps, and wire ARCore/ARKit for AR overlays.
4. **Wire data**: Bind Firestore streams (parking availability, crowd density) into exported widgets.
5. **Navigation & state**: Hook up routing/state management (e.g., `go_router`, Riverpod/Bloc).
6. **QA**: Device tests in a pilot venue; tune AR pose, lighting, and route snapping accuracy.

## 8. Risks & Mitigations
| Risk | Mitigation |
| --- | --- |
| Minor styling quirks after export | Keep Figma components tidy; schedule quick refactor passes post-export. |
| Re-export churn when designs change | Export screen-by-screen; rely on Git diffs; isolate UI packages. |
| Team learning curve on the plugin | Pair a designer + developer for first exports; document best practices. |
| AR calibration complexity | Reserve buffer time for anchoring, lighting adjustments, and route validation. |

## 9. Success Criteria
- **UI velocity**: 80% of non-AR screens generated via export with ≤20% manual fixes.
- **Fidelity**: Visual diffs vs. Figma within ≤2 px on core screens.
- **Integration**: Firebase streams render on exported screens within two days of export.
- **Stability**: AR route overlays stay within agreed drift thresholds at target venues.

## 10. Executive Summary (Arabic)
القرار: استخدام Builder.io (Visual Copilot) لتصدير واجهات Flutter مباشرةً من Figma، مع ربط Firebase وSitum وARCore/ARKit عبر كود Flutter أصيل.  
السبب: أسرع مسار من التصميم إلى الكود بجودة عالية ومن دون قيود، مع إبقاء التكاملات الحساسة (الملاحة الداخلية والواقع المعزز) تحت سيطرة فريق التطوير في Flutter.  
النتيجة: تقليل زمن بناء الواجهات، تحسين دقة التصميم، وتركيز الجهد الهندسي على وظائف الواقع المعزز والملاحة الداخلية والبيانات اللحظية.

## TL;DR
- Use Builder.io Visual Copilot for faithful Figma → Flutter UI export.
- Keep Firebase, Situm, and AR logic in native Flutter code for full control.
- Deliver feature velocity, design fidelity, and technical robustness for MSARE.
