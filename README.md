MJ Stack Tracker — Integrated Phase 9 Candidate
Package version: 0.9.0 (versionCode 9)
Platform: Native Android, Java + Android Views
Runtime: Offline; user selects a previously recorded local video
Build: GitHub Actions (build-phase9-integrated.yml)
Starting model: MASTER-001, initial count 416, nominal thickness 0.33 mm per piece
Why this build replaces the separate phases
This package consolidates the working Phase 0/1 video-player foundation and the Phase 3 analysis/history foundation, then brings the Phase 2/3.1 work-area and master-lock corrections together with calibration, lineage events, a conservation ledger, review/edit/delete, export, and the conceptual layer model. It is one integrated candidate source package rather than separate phase projects. The app uses package ID com.mj.stacktracker.phase9 so it can be installed alongside the older Phase 3 build during validation, avoiding an installation-signature conflict or deleting its local history.
Main workflow in the app
Select a recorded video from the phone. The video is displayed in a YouTube-like viewport.
Pause at the frame where the original, intact stack is visible.
Press MARK WORK AREA and drag a rectangle around the full region where the stack components can move. Keep unrelated bright objects and background decorations outside it.
Set the initial piece count (default 416) and nominal piece thickness (default 0.33 mm).
Press LOCK INITIAL STACK and drag one rectangle around the entire original stack. This stores the stable root identity MASTER-001 and the video timestamp from which analysis begins.
Press ANALYZE FRAMES. The OpenCV contour proposal detector works only inside the work area. Visual components use child IDs such as M01-P01, M01-P02, etc. Candidate split events are pending review; image detections never silently change piece counts.
Use the event/ledger controls to confirm splits/merges and manually record known piece counts, occlusion, out-of-view, additions, and confirmed removals. The ledger checks the conservation equation.
Calibrate a known line on the table plane and a separate known line on the stack side if physical estimates are needed. Recalibrate if the camera angle, zoom, crop, orientation or distance changes.
Review history by selecting a saved session. Search, tap a saved event to load its source video and seek to that event timestamp, confirm/reject/delete the event, and export a JSON or CSV report to a chosen local file.
What is actually implemented
Native Android video picker and Media3 playback.
Frame-by-frame analysis loop using one retrieved bitmap at a time; no full-video RAM load.
User-marked work-area rectangle, saved per video.
User-marked initial master-stack rectangle, saved per video, with an explicit MASTER-001 lineage root and lock timestamp. The first successfully decoded analysis frame is seeded from the user's manually marked master rectangle rather than relying on the weak contour detector to rediscover it.
OpenCV bright/neutral contour proposals restricted to the marked work area; overlapping proposals are suppressed.
Temporal visual ID association, three-frame persistence gate, pending possible-split events, and track-not-visible events.
Timestamp-indexed local SQLite history, progress saves, review overlay, local event-status editing/deletion, history search, JSON/CSV export.
Separate table-plane and stack-side pixels-per-millimetre calibration controls.
Local conservation ledger with visible, occluded, out-of-view, confirmed removed, added and unassigned buckets; split/unknown allocation actions and undo history.
Schematic digital model of 416 conceptual layers at a nominal 0.33 mm each. The model tilt slider is manual and illustrative.
JVM unit tests for ledger conservation and basic measurement math.
No camera permission, broad storage permission, Firebase, login, analytics SDK, runtime server or cloud AI API.
Important limits — not hidden
This is an integrated Phase 9 candidate, not a validated production tracker. The existing detector is still classical OpenCV contour logic, not a trained semantic model. A white region inside the work area can still be a false detection. Master locking provides a stable lineage root; it does not magically guarantee that every identical split component is visually identifiable after arbitrary motion or occlusion.
MASTER-001 is the stable logical root. M01-Pxx labels are visual component tracks. A new child ID is only a possible split, never proof by itself.
The initial 416 count is user-provided. The app does not prove that 416 physical pieces are visible.
Phase 9 uses a separate Android package ID from the earlier Phase 3 APK so both can coexist. The older Phase 3 app's local database is not automatically migrated into the Phase 9 app; it remains in the old app until you choose to remove it.
The ledger is conservative: ambiguous child counts move into unassigned instead of inventing counts. Automatic detections cannot silently modify the ledger.
Confidence is a heuristic tracking score, not measured accuracy. Accuracy must be established with labelled ground-truth videos.
Plane/side calibration produces local projected scale. It does not by itself calculate a physically correct 3D pose, correct all perspective effects, or guarantee sub-millimetre height accuracy.
The 416 layer drawing is a conceptual schematic. Display tilt is manually selected; automatic video-derived 3D pose is not implemented/verified in this candidate.
Frame timestamps are estimated from available indexed-frame count and video duration. Variable-frame-rate media may need a later MediaExtractor/MediaCodec presentation-timestamp decoder for stricter timestamp accuracy.
Split/merge detection remains candidate-level. Count allocation is manual and must be checked by the user. No 100% accuracy claim is made.
Build from phone using GitHub Actions
Upload these files once:
MJ_STACK_TRACKER_PHASE9_INTEGRATED_SOURCE.zip to the repository root.
README.md to the repository root (this file; replace the old README).
build-phase9-integrated.yml to .github/workflows/build-phase9-integrated.yml.
Then open Actions → Build MJ Stack Tracker Phase 9 Integrated → Run workflow. The workflow validates the ZIP contents, extracts the project, runs testDebugUnitTest, builds assembleDebug, verifies the APK, and uploads build diagnostics.
Expected artifact: MJ-Stack-Tracker-Phase9-debug-apk; APK path inside artifact: app-debug.apk.
The workflow pins the Android build versions used by this project: AGP 9.4.0, Gradle 9.6.0, JDK 17, compile SDK 36. See the official AGP 9.4.0 release notes for compatibility.
Phone test checklist after a successful build
Confirm app title says Integrated Phase 9, not Phase 3.
Confirm MARK WORK AREA, LOCK INITIAL STACK, and the count/thickness inputs are visible.
Load a test video, mark the table movement area, pause on the intact initial stack, then lock it.
Run analysis for a short clip. Verify that the first candidate is sought inside the master rectangle and child IDs use M01-Pxx.
Verify that a visible candidate/split does not change the ledger automatically.
Test an unknown split: the piece count should move to unassigned, preserving the conservation equation.
Test export, history review, event confirm/reject/delete, and app restart.
Record false detections, missed tracks, ID switches, pending events, processing speed and memory use before using the model on longer videos.
