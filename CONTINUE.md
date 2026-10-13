# Lantern English — learning upgrade checkpoint

Saved on 2026-10-13. Work stopped at the owner's request because the session limit was approaching. This is an unfinished development checkpoint, not a production release.

## What is in the archive

`lantern-learning-progress.zip` contains a `site/` directory with the changed source files, preserving their website paths; overlay these onto the production baseline. The `checks/` directory contains the local test harness and implementation scripts. Do not run `upgrade.mjs` blindly: it was a one-time transformation against a previous local snapshot.

The production baseline is main commit `34d0a224066ac347386cd161f93e6d5a8fb87886`. That release already contains 18 additional videos (9 B2, 7 C1, 2 C2), with 31 videos overall, and removes visible content and notebook counters. Preserve all of those changes.

## Owner's request

Make the website more professional: learning flow, richer activities, better video discovery and visuals, consistent level guidance, backup/transfer and maintainability/performance. Most importantly, tapping words in YouTube film subtitles should show their Turkish meanings. Never restore visible totals for stories, articles, videos, dictionary entries or notebook words.

## Prepared changes

- New `assets/js/learning.js` module, integrated into `app.js` and `index.html`.
- YouTube IFrame API player with a separate synchronized clickable transcript. It supports local English SRT/VTT import, timing offset, dictionary and notebook integration, pause on word tap, previous/next cue, repeat until cue end, and resume. Imported subtitles stay in browser storage.
- IMPORTANT: YouTube's own CC captions cannot be read by the embedding page. No built-in transcript was obtained for the ten YouTube films. The implementation currently requires a learner-provided subtitle file for those films. Do not claim automatic, ready-to-use clickable YouTube captions are complete. To finish that request fully, obtain usable English subtitle files with appropriate publication rights or select officially reusable content. The ordinary YouTube caption download API requires authorized access. Do not fabricate dialogue or present original exercises as transcripts.
- Search, CEFR, duration and subtitle-readiness filters; clear/reset and empty-state handling.
- Actual YouTube thumbnails and distinct typographic category covers for native videos.
- Personal study-plan route, level/time/type preferences, recommendations, continuation from saved reading/video positions.
- Video comprehension questions for selected titles, glossary meaning practice, reflection tasks with local saved notes, completion status. Reading pages also gain practice/reflection sections (existing reading quizzes remain).
- JSON backup/export/import for notebook, reading progress, bookmarks, video progress, learning notes and study preferences. Validated schema, merge and quota rollback; excludes downloaded videos and custom subtitle files. No cloud account/synchronization.
- Explicit CEFR estimate guidance and data/privacy explanations.
- Native captions moved out of `data/videos.js` to lazy-loaded `data/video-captions.js`; catalogue loads separately. Existing native captions are preserved. Service worker caches the new learning module; captions file is fetched/cached when needed, not precached on first visit.
- Responsive CSS appended to `assets/css/style.css`.

## Validation completed

`node --check` passed for app.js and learning.js.

`check-learning.mjs` passed locally with Node and linkedom. It checks all 31 video routes, learning panel rendering, dictionary popup, video filters and empty/reset states, SRT/VTT parsing and invalid input rejection, YouTube API synchronization using a fake player, pause/replay/navigation/cleanup, reflection persistence, reading/study routes, backup validation/merge/prototype-key rejection/quota rollback, and absence of forbidden counters across all routes.

These tests use a simulated DOM/player. Real YouTube playback, subtitle upload, mobile layout and live deployed behavior have NOT been verified for the new version. The current production site was verified during the previous completed task.

## Resume checklist

1. Extract the archive; compare with current GitHub main in case the owner made later changes.
2. Review the large new learning module and improve organization if needed. It was prepared quickly and has not had a full final code review.
3. Fix word-source linkage in app.js: `toggleSave` still uses `src: text && text.id && BY_ID[text.id] ? text.id : null`; include VIDEO_BY_ID so saved video words link back. The notebook display lookup was already extended to videos, but this save-side condition was not fixed before stopping. Phrase-save has the same story-only condition.
4. Review native resume behavior if the learner explicitly selects a cue before first playback, and loading-route race/cleanup cases.
5. Review page titles for new #study and #backup routes, and placement of the reading practice section after existing end content.
6. Finish the subtitle-content requirement honestly; current YouTube importer is working infrastructure, not complete built-in film transcripts.
7. Run meaningful tests, then real browser desktop/mobile checks (including backup import and YouTube file upload) before publishing.
8. Add a maintainable content/link validation script or workflow; automatic external-link maintenance has not yet been implemented. Teacher editorial review and cloud sync are also not done.
9. Publish the finished source files in their real paths and refresh the service worker version. Verify GitHub Pages deployment and live behavior, then report remaining limitations accurately.

## Environment and publishing

Working directory was `C:\Users\emin\Documents\Codex\2026-10-13\h`.
Prepared repository: `work/lantern-repo`.
Local test command: `node work/check-learning.mjs`. The test imports `work/qa-dom/node_modules/linkedom/esm/index.js` and reads `work/lantern-repo/`; adjust those paths if relocating. Linkedom was installed in the local QA folder; dependencies are not included in the checkpoint archive.

GitHub connector read access works, but writes to `lantern-english/lantern-english.github.io` previously returned 403 (integration access). The authenticated GitHub browser session is `emjnbulut` and supports web uploads. Use existing authorized browser access without expanding permissions. This checkpoint is saved separately from production; do not merge a zip archive into the website as a feature release.

