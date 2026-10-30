MJ Stack Tracker — Phase 10.1 Tabletop ROI
Purpose
Phase 10.1 is a targeted correction to Phase 10 for the supplied 416-card stack video.
The build focuses on:
restricting visual candidates to the tabletop region;
filtering background, table text and unrelated objects;
initializing the original 416-card stack as PILE-01 only when the opening-stack conditions match;
protecting the original pile identity after a split;
producing diagnostics for pile candidates and analysis results.
Important limitation
This phase is not yet a final accuracy release.
The following are not claimed as fully verified:
exact 1-card quantity accuracy;
exact 20–25-card transfer accuracy;
complete source-to-destination transfer matching;
accuracy across arbitrary videos.
The supplied test video is used to validate pile-region detection and the analysis pipeline first.
Repository upload
Upload these files:
MJ_STACK_TRACKER_PHASE10_1_TABLETOP_ROI_SOURCE.zip
Repository root
build-phase10-1-tabletop-roi.yml
.github/workflows/build-phase10-1-tabletop-roi.yml
README.md
Repository root
Do not delete the previous Phase 10 files until the new build has been verified.
GitHub Actions
Open:
GitHub → Actions → Build MJ Stack Tracker Phase 10.1 Tabletop ROI → Run workflow
After the workflow succeeds, download the generated APK artifact and install it on the test phone.
Test procedure
Use the same supplied test video first.
Check:
Video selection works.
Playback works.
Analysis completes.
The opening stack is recognized as the starting pile.
The split produces separate pile candidates.
The final part of the video can identify up to four visible pile regions.
No unrelated background object is reported as a pile.
Transfer History is populated only when a transfer can be supported by the available visual evidence.
Interpretation of results
A successful Android build only proves that the source compiles and the APK is generated.
A successful video analysis does not automatically prove exact card counts.
For accuracy validation, compare:
detected pile count;
relative pile measurements;
inventory conservation;
transfer events;
expected physical card movements in the test video.
Project constraints
Native Android application.
Offline runtime.
No Firebase or cloud AI runtime dependency.
Video is selected from the phone; camera permission is not required.
GitHub Actions is used for cloud compilation/testing only.
Starting inventory: 416 cards.
Version
Phase 10.1 — Tabletop ROI / pile-detection correction Application version: 1.0.1 Version code: 17
