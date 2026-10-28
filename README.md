MJ Stack Tracker

Offline Android video-based stack tracking and quantity measurement app.

Project Status

🚧 Development — Phase 0

MJ Stack Tracker is being developed in small, testable phases.

The first goal is to prove that the Android project can build successfully through GitHub Actions, install on an Android phone, and launch correctly.

«Important: A successful APK build does NOT mean automatic one-piece counting is already solved. Counting accuracy must be demonstrated using controlled videos with known quantities.»

---

Project Goal

MJ Stack Tracker is designed to:

- Select an already-recorded video from the phone.
- Process the video locally on the Android device.
- Track visually similar pieces and stacks.
- Maintain an inventory starting from 416 pieces.
- Use 0.33 mm as an editable nominal thickness input.
- Track stack movement, removal, addition, transfer, split and merge events.
- Show red side/edge overlays and blue top-surface outlines.
- Estimate quantity only when sufficient visual evidence exists.
- Mark uncertain results as Pending Confirmation instead of inventing an exact count.
- Keep an editable local event history.
- Support manual correction, undo/reversal and CSV/JSON export.
- Work offline at runtime.

---

What the App Does NOT Require

- No camera permission.
- No Firebase.
- No login/account.
- No cloud AI API.
- No runtime server.
- No uploading of original videos to GitHub or cloud AI.
- No OCR/card-rank recognition in the initial stack-counting system.

---

Initial Inventory Model

- Starting inventory: 416 pieces
- Default nominal thickness: 0.33 mm/piece
- Theoretical total thickness: 137.28 mm
- Thickness is a configurable input, not a proven measurement from the video.

The app must never force visible stacks to equal 416 when some pieces are hidden, temporarily occluded, out of view or confirmed removed.

---

Core Principle

Movement is NOT automatically a quantity change.

The system must distinguish:

- Stack movement
- Perspective/scale change
- Camera movement
- Blur
- Hand occlusion
- Stack lift/rotation
- Split
- Merge
- Transfer
- Pieces leaving the visible area
- Actual quantity change

Automatic events should be confirmed only when multiple frames and consistency checks provide enough evidence.

If the evidence is insufficient, the result must be marked:

Pending Confirmation / Unknown

instead of creating a fake exact count.

---

Development Phases

Phase 0 — Build & Install Proof

- Minimal Java Android app.
- GitHub Actions build.
- Debug APK artifact.
- Install on Android phone.
- Launch successfully.

Acceptance

- Build succeeds.
- APK artifact exists.
- APK installs.
- App launches.
- Known-good commit is preserved.

---

Phase 1 — Video Input & Player

- Android Photo Picker / SAF.
- Select local video.
- Play/pause.
- Seek.
- Timestamp and duration.
- Video remains synchronized with analysis.

---

Phase 2 — Overlay & Manual Calibration

- Red side/edge lines.
- Blue top-surface outline.
- Adjustable anchor points.
- Manual correction.
- Calibration saved per video session.
- Recalibration option.

---

Phase 3 — Frame Analysis

- Bounded frame extraction.
- Original-frame preservation.
- Blur/sharpness measurement.
- Optional denoise.
- Contrast enhancement.
- Conservative sharpening.
- Original vs enhanced comparison.

The entire 4K video must never be loaded into RAM.

---

Phase 4 — Geometry

- Table-plane perspective correction.
- Separate raised-stack side/height measurement.
- Detect calibration drift.
- Detect camera movement where possible.
- Detect low-confidence views.
- Request recalibration when necessary.

Moving a stack closer/farther from the camera must not automatically become a quantity change.

---

Phase 5 — Stack Tracking

- Stable Stack IDs.
- Position tracking.
- State tracking.
- Occlusion handling.
- Out-of-view handling.
- Split detection.
- Merge detection.
- Transfer detection.

---

Phase 6 — Quantity Estimation

The quantity engine may use:

- Stack side profile.
- Apparent physical height.
- Local calibration.
- Visible layer/edge periodicity when genuinely resolved.
- Temporal consistency.
- Known starting quantity.
- Configurable nominal thickness.

The system must produce:

- Candidate quantity.
- Confidence.
- Evidence/state.
- Pending status when uncertain.

A single screenshot or AI guess is not sufficient proof of an exact quantity.

---

Phase 7 — Inventory & History

Local database must store:

- Video session.
- Calibration.
- Stack IDs.
- Current quantity estimate.
- Stack position.
- Stack status.
- Confidence.
- Events.
- Before/after counts.
- Event delta.
- Timestamp.
- Confirmation state.
- Corrections.

Required actions:

- View event.
- Jump video to event timestamp.
- Confirm.
- Correct.
- Undo/reverse.
- Export CSV.
- Export JSON.

Corrections must preserve an audit trail.

---

Phase 8 — Robustness Testing

Test:

- Fixed stack.
- No movement.
- 1-piece change.
- 5-piece change.
- 10-piece change.
- Larger quantity changes.
- Stack movement.
- Transfer between stacks.
- Split.
- Merge.
- Hand occlusion.
- Stack lift.
- Stack rotation.
- Out-of-view.
- Return to view.
- Blur.
- Lighting changes.
- FHD video.
- 4K video.
- Long videos.
- Unsupported/corrupt videos.
- Low-storage conditions.

---

Phase 9 — Release Candidate

- Freeze dependencies.
- Clean install test.
- Update test.
- History persistence test.
- Debug verification.
- Signed release build.
- Final feature checklist.
- Accuracy report.
- Known limitations document.

---

Computer Vision Rules

The untouched original frame is the primary measurement source.

Enhanced frames are supporting observations only.

Filters such as:

- Denoise
- Contrast
- Sharpening
- Stabilization
- Super-resolution

must not be treated as proof of physical information that was not actually recorded.

AI super-resolution must NOT be the sole source for exact quantity counting.

---

Accuracy Policy

The project must measure:

- Exact-count accuracy.
- Absolute count error.
- 1-piece change accuracy.
- 5-piece change accuracy.
- 10-piece change accuracy.
- False change events per minute.
- Missed events.
- Stack tracking/recovery rate.
- Processing time.
- RAM usage.
- Crash behavior.
- Percentage of events requiring manual confirmation.

The project must NOT claim:

"100% accurate"

unless representative known-ground-truth testing actually supports that claim.

---

Recommended Technology

Initial technology:

- Native Android.
- Java for Phase 0.
- AndroidX Media3 / ExoPlayer.
- Android Photo Picker / Storage Access Framework.
- OpenCV when required.
- SQLite initially.
- Background Executor/threading.
- GitHub Actions for cloud compilation.
- Offline runtime.

Dependencies must be added gradually.

Do NOT add video, OpenCV, AI, OCR and database together in the first build.

---

Phone-Only Development

The project is designed to be developed from an Android phone.

Normal workflow:

1. Edit/upload files through GitHub.
2. Commit changes.
3. Start GitHub Actions.
4. GitHub builds the APK in the cloud.
5. Download APK artifact to the phone.
6. Install APK.
7. Test on the phone.
8. Record test result.
9. Continue only after the current phase passes.

A laptop/PC is not required for the normal development workflow.

GitHub Actions is used for building, not for running the AI or processing the user's video.

---

Repository Structure

Phase 0 uses:

MJ-Stack-Tracker/
├── README.md
├── MJ_Stack_Tracker_PHASE0_SOURCE.zip
└── .github/
    └── workflows/
        └── build-phase0.yml

The Phase 0 ZIP must contain the Android project directly at its archive root:

build.gradle
settings.gradle
gradle.properties
app/
├── build.gradle
└── src/
    └── main/
        └── AndroidManifest.xml

There must NOT be an unnecessary nested parent folder inside the ZIP.

After the first successful baseline, a standard Gradle Wrapper should be committed and later builds should use it directly.

---

Build Safety Rules

DO NOT:

- Run Gradle setup before extracting the source archive.
- Mix multiple old Phase 0 workflows.
- Use different source ZIP names for the same active workflow.
- Add all dependencies at once.
- Treat YAML validation as proof of a successful Android build.
- Treat ZIP validation as proof of a successful Android build.
- Treat APK compilation as proof that automatic counting works.

When a build fails:

1. Find the first meaningful error.
2. Identify the actual root cause.
3. Fix only the relevant problem.
4. Rebuild.
5. Test the APK.
6. Preserve the last known-good commit.

---

Previous Problems to Avoid

Previous development encountered:

- Incorrect/nested ZIP project paths.
- Gradle setup before project extraction.
- Workflow not appearing in GitHub Actions.
- Missing Gradle Wrapper.
- Kotlin "singleLine" errors.
- Kotlin "LayoutParams" errors.
- Unnecessary ML Kit native-library packaging problems.
- Too many dependencies added too early.

Phase 0 intentionally uses simple Java Android Views to reduce these risks.

---

Testing Requirement

The current sample video alone is NOT sufficient to prove exact counting.

Controlled test videos must be created with known ground-truth changes.

Example:

Initial Stack = 100

Remove 1  → expected 99
Remove 5  → expected 94
Add 10     → expected 104
Transfer 5 from Stack A → Stack B
Split stack
Merge stacks
Hide stack → Unknown/Occluded
Return stack → Reconcile

The ground-truth event log should be maintained separately from the application so the algorithm can be objectively evaluated.

---

Privacy

All video analysis is intended to happen locally on the phone.

Original videos must NOT be uploaded to:

- GitHub
- Firebase
- Cloud AI services
- Remote servers

GitHub Actions is only a build environment.

---

Current Milestone

Phase 0

Checklist:

- [ ] GitHub repository created.
- [ ] Correct Phase 0 source ZIP uploaded.
- [ ] "README.md" added.
- [ ] Workflow exists at ".github/workflows/build-phase0.yml".
- [ ] "workflow_dispatch" works.
- [ ] GitHub Actions build succeeds.
- [ ] APK artifact is generated.
- [ ] APK installs on Android phone.
- [ ] App launches successfully.
- [ ] Known-good commit is preserved.

Do not start Phase 1 until Phase 0 passes.

---

Development Philosophy

Build → Compile → Install → Test → Measure → Preserve → Continue

The project prioritizes measured results over assumptions.

Automatic quantity counting is an engineering goal. It must be demonstrated using real controlled test footage before it is considered reliable.

MJ Stack Tracker — Build small. Test honestly. Measure accurately.
