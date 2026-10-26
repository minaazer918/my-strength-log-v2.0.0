# Changelog

## 2.0.0 - 2026-10-26
### Changed
- The app now starts with a clean slate: no exercises are built in. Users add the exercises they want to track. Squat and Deadlift are ordinary exercises like any other.
- Existing installs keep their data: workouts saved as `squat` / `deadlift` are recognised automatically and appear as "Squat" and "Deadlift" on Home. No workout record is rewritten and the IndexedDB structure (`strengthlog` v1, `workouts` store) is unchanged.
### Added
- Add exercise screen (Home and Settings): type a name or pick from suggestions, plus a movement type (Squat / Hinge / Push / Pull / Core / Other). Duplicate names are blocked (ignoring capitals and extra spaces); typed names are escaped everywhere they are shown.
- Empty Home shows quick-add buttons (Squat, Deadlift, Bench Press, Overhead Press, Barbell Row).
- Settings > My exercises: Hide / Show an exercise. Hidden exercises keep all history.
- History uses a dropdown filter when there are more than 2 exercises.
- Exercise list is stored in localStorage (`exercises`); suggestions live in the new file `exercise-presets.js` (ids are permanent).
- JSON export (file version 2) now includes the exercise list. Import accepts old v1.1.0 exports and recreates exercises from the workouts in the file; an exercise id with no known name appears as "Unknown exercise".
### Files
- app.js, index.html (script tag, footer `v2.0.0`), sw.js (cache `strength-log-v4`, new file cached), manifest.json and README.md (neutral wording), new exercise-presets.js, CHANGELOG.md. style.css and the icons are unchanged.

### Test results
Verified (headless Chromium, 390 px wide, project served over localhost, service workers blocked; 23 of 24 checks passed, see the failure below):
- `node --check` passes for app.js, sw.js and exercise-presets.js.
- Upgrade: test workouts written by v1.1.0 are byte-identical after loading v2.0.0; Home shows SQUAT and DEADLIFT; History shows both; the Squat form shows the previous workout; the kg unit setting is kept.
- Fresh install: empty Home with prompt; quick-add; typed "squat" matches the suggestion; duplicate, empty and `<img onerror>` names are handled safely; workouts save for added exercises; History dropdown filters; hiding an exercise removes it from Home but keeps its history.
- Export includes exercises; an old-format file imports into a clean profile and recreates Squat, Deadlift and an "Unknown exercise" fallback; a v2 export round-trips; the list is rebuilt from workouts if localStorage is cleared.
- No page errors were logged. style.css and the icons are byte-identical to the uploaded files.
- One check failed: the footer text in the upgrade test, because the test browser reused its cached v1.1.0 index.html. In a clean browser the footer shows `v2.0.0` and both scripts load.

Not verified:
- Upgrade of an already-installed app on the real Samsung phone (service worker v3 to v4), appearance on the phone and in dark theme.
- Edit, delete, kg conversion and rest timer after the change (code not changed, not run).
- Suggestion link/form, similar-name warning, merging and category-based features are not included.

## 1.1.0 - 2026-10-10
### Added
- App version (`v1.1.0`) is now shown unobtrusively in a footer at the bottom of every screen.
  - index.html: added a footer element (id `ver`) below `#app`, styled with the existing `--mu` color variable.
  - sw.js: cache name changed from strength-log-v2 to strength-log-v3 so installed apps load the updated index.html.
  - app.js, style.css and the database structure are unchanged; no workout data is touched.
- Version is kept in sync manually: update the footer text in index.html together with this changelog on each approved release.

### Test results
Verified (headless Chromium, 390 px wide phone-size window, project served over localhost):
- `node --check` passes for sw.js and app.js.
- Footer shows `v1.1.0` and is visible on Home, Squat entry, History, Settings and the "Workout saved" screen.
- Saving a one-set test workout still works ("Workout saved" appears) and no page errors were logged.
- After load, the only cache present is `strength-log-v3`.
- Screenshot of the saved screen checked: footer is small, muted and centered below the buttons.
- app.js, style.css, icon-192.png and icon-512.png are byte-identical to the uploaded 1.0.1 files (checksums match).

Not verified:
- Appearance on the real Samsung phone, and in dark theme.
- That an already-installed app upgrades from v2 to v3 and shows v1.1.0 after deployment.
- Edit, delete, import/export, kg mode and rest timer after the update (no code involved changed, but not run).

## 1.0.1 - 2026-10-10
### Fixed
- Individual sets can now be saved, so you can alternate between Squat and Deadlift.
  - app.js: blank unsaved trailing sets are skipped on save; opening an exercise resumes today's workout if it has fewer than 4 sets.
  - sw.js: cache name changed from strength-log-v1 to strength-log-v2 so installed apps load the new app.js.

## 1.0.0 - initial version
- First release.
