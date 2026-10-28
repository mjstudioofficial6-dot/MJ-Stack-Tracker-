MJ Stack Tracker — Step A (Consolidated Mobile-Build Bundle)
Language: Java Android app
Package: com.mjthakur.stacktracker
Runtime: offline/local; no camera permission and no Internet permission
Build route: GitHub Actions from an Android phone
What this bundle currently includes
Local video selection via Android's document picker.
Video playback, pause and seek controls using AndroidX Media3 ExoPlayer.
An adjustable overlay: red side/edge lines, blue top-surface lines and movable corner handles.
Save/reset overlay calibration on the phone.
A logical 416-piece model and assumed 0.33 mm per piece.
Local SQLite stack ledger with Deck 1, additional stacks and an Out of View / Unassigned bucket.
User-confirmed transfer of a known number of pieces between stacks; transfers conserve the total quantity.
Local event history.
Current-frame extraction plus a relative edge-detail comparison between the original frame and a grayscale/contrast/sharpen analysis copy.
GitHub Actions workflows, a structural validation script, requirements, architecture and test plan.
Critical status notice
This is a Step A build foundation, not yet a validated automatic single-piece counter. Automatic stack detection, perspective-aware physical height measurement, automatic layer-edge matching, automatic split/merge event detection and confidence-calibrated quantity changes are not implemented in this build. The UI explicitly avoids calling the relative frame-detail score a count or confidence score.
A real test clip plus a controlled test where the actual removed count is known is required before claiming automatic one-piece accuracy. A fixed camera and a 416-layer digital model cannot recover edges that are not resolved in the encoded video.
Build configuration (pinned)
Android Gradle Plugin: 9.4.0
Gradle distribution: 9.6.0 (installed by GitHub Actions; this archive intentionally uses no Gradle Wrapper JAR)
JDK: 17
Android compile/target SDK: 36
Minimum Android SDK: 26
AndroidX Media3: 1.11.1
The versions above were selected to match the official AGP 9.4 compatibility notes and Media3 stable release information as checked on 28 October 2026. If a repository's runner or dependency mirror has different availability, fix the build error from the first real failing line rather than randomly changing versions.
Easiest phone-only GitHub method
This is recommended when using GitHub in a mobile browser and you do not want to upload dozens of files individually.
Create a new Private repository named MJ-Stack-Tracker at https://github.com/new.
Upload the file MJ_Stack_Tracker_StepA.zip to the repository root. Keep the filename exactly the same.
In GitHub choose Add file → Create new file. In the filename/path box enter .github/workflows/build-apk.yml.
Copy the contents of the separately supplied build-apk-from-zip.yml into that file and commit it to main.
Open the repository's Actions tab, choose Build APK from uploaded source ZIP, then press Run workflow if it has not started automatically.
When the run succeeds, open the run summary and download artifact MJ-Stack-Tracker-StepA-debug-APK.
Extract the artifact ZIP on the phone, install app-debug.apk, and test with a non-sensitive short sample video.
When you receive a code update, replace MJ_Stack_Tracker_StepA.zip in the root with the new ZIP and commit. The workflow will rebuild it.
Alternative: normal Android repository layout
Extract this source ZIP on the phone.
Upload the extracted project contents (including .github, app, settings.gradle, and build.gradle) to the repository root.
The included .github/workflows/build-apk.yml will build from the repository root.
Screens and controls
Open Video: select a local video file.
Adjust Lines: show corner handles and drag them to align the overlay with the object. This is annotation calibration, not a fully automatic detector.
Save Lines / Reset: store or restore the overlay points locally.
Check Current Frame Quality: analyze the current playback time on the phone. The original/filtered scores are affected by edges, noise, compression and sharpening; they are only relative detail indicators.
New Stack: create a stack record with zero pieces.
Transfer Pieces: transfer an explicit amount from a source stack to another stack. Transfers are manual in Step A.
Out of View / Unassigned: account for pieces that are hidden or not currently visible; occlusion is not treated as removal.
Reset Inventory: start a new 416-piece baseline. Existing history remains and gets a reset event.
Current calculation
416 × 0.33 mm = 137.28 mm = 13.728 cm
This is an arithmetic result using the assumed per-piece thickness. It does not validate that the actual physical stack measures this height.
Validation before automatic counting
Read docs/TEST_PLAN.md and record known quantities independently. At minimum test 0, 1, 5, 10 and 20-piece changes, table-position changes, split/merge, hand occlusion, blur, compression and off-screen movement. Keep automatic quantity mutation disabled until results are trustworthy.
Project files
app/: Android source.
.github/workflows/build-apk.yml: direct repository build workflow.
scripts/validate_project.py: structural sanity checks run before Gradle.
docs/REQUIREMENTS.md: scope and fixed assumptions.
docs/ARCHITECTURE.md: processing architecture and constraints.
docs/TEST_PLAN.md: build and measurement acceptance tests.
Privacy and safety
No video upload, account, remote AI, network permission or camera permission is included. The selected video is read through the Android document provider. Do not commit user videos, credentials, signing keys or private event exports to GitHub.
