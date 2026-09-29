# UTZLINE Site Measure — installable app

**Current version: v68 (RC 1.0)** (bump this line, and add a dated changelog
entry below, every time a new build ships — see `next-version-notes.md`
in the project for the full per-version changelog; v40 through v45.6
shipped without this README's own version line being kept in sync, so
that file is the authoritative record for that stretch.)

**v68 (2026-09-29) — RC 1.0.**

- **The saved copy's title block** (Save PDF / PNG, backups, the current-view snapshot). Andrew: *"this text must be scalable, it comes out great on an a1 size but when smaller or shapshot it takes over the whole title block area, it needs to have the room name, joinery name and date time username, all in readabele. multi row text."*
  - It's now rows of text: **Joinery** code (larger), **Room**, level · project, then **Saved** date, time · who.
  - All of it is sized as a share of the copy's own width (2.4%, capped for very big sheets). A snapshot's block is the same proportion of the picture as an A1's, not the whole bar.
  - Each row shrinks a little to fit, then is cut with "…". The logo sits on the right, as tall as the rows.
- **Site measure not required** (right-click a joinery item). Andrew: *"on right click have another option for site measure not required, this flags it as check measured (not required)"*. It's the same Check measured step (📏), with `measureNotRequired` and a note on the event, so every app shows it as check measured; UTZLINE Projects' History reads "Check measured — site measure not required".
- **Save & exit on a check measure** asks **"Mark check measure complete?"** with **Yes** / **No** (Andrew: *"is check measure complete should be mark check measure complete, with yes or no buttins"*).
- **Insert image → a PDF:** each page picked gets the same crop as a photo, one at a time ("Use whole page" keeps it as it is). Andrew: *"insert a file still needs the crop capability"*.

**v67 (2026-09-29) — RC 1.0.** Andrew: *"ok, now change them all to version RC 1.0. and have that on the logos (small)"*.

- The app is now **RC 1.0** (release candidate 1.0) across the UTZLINE family. A small **RC 1.0** tag sits beside the logo in the toolbar and on the start screen, and the quick-help tip reads "RC 1.0 (build v67)".
- The build number (v67) still counts up underneath, so installed copies pick up each update. It's also what the Windows installer "Setup RC 1.0" contains.

**v66 (2026-09-29):** **Timings removed.** Andrew: *"remove timings"*.
- The **⏱ Timings** button on the Projects screen and its list are gone. Nothing about opens is stored on the device any more, and the old list is cleared.
- **Plan PDFs read offline.** The pdf.js worker was still loaded from the internet (only `pdf.min.js` was local), so reading a plan PDF needed a connection. It now uses the local `pdf.worker.min.js`, which was already in the folder and precached.
- This build is also the one inside the new Windows installer.

**v65 (2026-09-29):** Same build as the Viewer's v65, where job notes added from the Viewer are stamped **FOR CONSTRUCTION**. Site Measure itself doesn't add job notes, so nothing changes here. Test: `pdftest-projects/run_v65_jobnote_for_construction.js`.

**v64 (2026-09-28):** Shop drawings. Andrew: *"shop drawings need a sent and a returned section"* and *"we call them REV A REV B and so on"*.
- View shop drawing now has a **Sent** part and a **Returned** part. Each drawing gets a revision picker (latest chosen) and Open.
- Returned copies come from the drawing's `Returned` folder; the Scheduler adds them.
- Revisions read as REV A, B, C…; files saved as REV 0/1/2 show as A/B/C, and the first-day "RevA" files still read.
- Tests: `pdftest-projects/run_v64_shop_drawings_sent_returned.js` (new) and `run_shop_drawings.js` (updated).

**v63 (2026-09-28):** Rework round. Andrew asked for every rework change to be its own file (*"also need to fix this rework conflict"*), a status log per rework, and *"reworks that are delivered to be green border / text and sent to bottom of page"*. The other apps now write each change as its own small file in the item's rework log folder, and this build reads them.
- The shared rework code (`shared/utzline-rework.js`, `window.UtzRework`) is now pasted into `source.html` by `shared/sync_rework_module.py`. Never edit that block by hand. Site Measure itself doesn't use it: rework stays Viewer-only (v60).
- The Viewer part is in the Viewer README.

**v62 (2026-09-28):** Button wording. Asked whether to change to "Create / Open site measure" or keep "Add / Open check measure", Andrew answered *"Stay"*, then *"Actually. Create"*.
- The joinery item's button and its long-press row now read **Create site measure** when there isn't one yet and **Open site measure** when there is. The rule for which one shows hasn't changed. The Viewer always says "Open site measure", and the Timings panel title matches.
- Nothing else is renamed. "Mark as check measured" stays as it is because it's a joinery status.
- Tests updated for the new labels.

**v61 (2026-09-28):** Two requests from Andrew.

1. **All check measures are layers.** Andrew: *"Check measure should always show all check measures. The point of the user layers is to be able to turn them off temporarily. Latest always to top."*
   - Every saved check measure is now a toggleable layer, drawn oldest to newest so the newest is on top. The layers panel lists newest first.
   - Site Measure with your own draft or save: that's your editable page (as before), and every other save is a layer.
   - Nothing of your own (and always in the Viewer): the page is the latest save's plan with no marks, and **every** save, the latest included, is a layer.
     - v60 copied the latest save's marks onto your page instead. That hid that save from the layers list and duplicated its marks into your next save.
     - That save's file is read once and reused for its layer.
   - If there's only someone's unsaved draft, it shows as a layer marked "unsaved".
2. **Viewer: Reworks screen.** Andrew: *"view rework in viewer app should open the reworks, and allow comments to be added like sent to saw with user time date logging. it needs to be like the photo"*, *"reworks that are delivered to be green border / text and sent to bottom of page (maybe a separate selectable delivered folder)"*, then *"Do the viewer rework also"*.
   - View rework (project menu, the item dialog, and the long-press row) opens a full screen like Install ITP's Outstanding reworks page.
   - Outstanding reworks are grouped by level and room, newest first. Each card shows the code and cabinet, a state pill (Logged / Cut / Manufactured), who logged it and when, the text, the latest comment, and **Show on plan** / **Open rework**.
   - Delivered and closed-out reworks sit in a separate **Delivered (N)** section at the bottom, tap to open, with a green border and text.
   - **Open rework** shows the photos (tap to enlarge) and the whole status log, newest first: logged, every state change, delivered, closed out, and every comment, each with name, date and time.
   - **Add a comment**, with a "Sent to saw" quick pick, needs a name and PIN.
   - Each comment is its own new file, named by who and when, in a log folder beside the rework file:
     - flat projects: `Project Saves/UTZLINE ITP/Install ITP Rework Log/<Level> - <Room> - <Code>/<name> - <date time> - comment.json`
     - legacy projects: `itp-install-rework/<Level>/<Room>/<Code> log/…`
   - The shared rework file (which Install ITP and Delivery ITP rewrite whole) is never written, so comments can't cause OneDrive conflict copies. Install ITP, Delivery ITP and the rework PDFs pick these comments up in their own parts of the rework round.
- Tests:
  - New: `run_v61_viewer_reworks.js`.
  - Updated for all-layers: `run_v60_open_existing.js`, `run_v54_check_measure.js`, `run_layers_discoverability_fix.js`, `run_joinery_item_dialog.js`.
  - The built-app fake folders can now create files. The full suite passes.

**v60 (2026-09-28) — important fix:** Andrew, on v59: *"when opening an existing site measure, it's bringing up the popup with the crop plan instead. we need to decipher if the button says create site measure or open site measure based on if there is one existing -- fix this important"*, and *"the view rework does not need to be in site measure app, remove it"*.

- **Cause:** the button followed "does anyone have a check measure for this item" (`checkMeasureKeySet`), but opening followed "does *this device's name* have one". So an item someone else measured, or one saved under a different name on this device, was treated as brand new: plan snapshot, crop popup and an empty page.
- **Fix: one rule for both** (`loadCheckMeasureBase` / `readCheckMeasureState`, read before leaving the plan):
  - **Exists** (any saved check measure, my draft, or anyone's unsaved draft): it opens straight in, with no crop popup. The page starts from my draft, else my latest save, else the latest save by anyone, else the newest unsaved draft by anyone. A toast says whose it opened from. Every other save is still a layer, and my Save becomes my own new layer without changing theirs.
  - **Doesn't exist:** "Add check measure", plan snapshot and crop, as before.
  - Names on this device are matched case-insensitively.
  - A folder that can't be read (mid-sync) is retried once, then reported with "still syncing? — nothing was changed" while you stay on the plan. It's never mistaken for "doesn't exist", so it can't fall back to the crop popup.
- **View rework is Viewer-only now:** the long-press row, the item dialog button and the project menu button are gone from Site Measure. Site Measure also skips the rework listing in its background scan.
- Timings step names changed ("read the check measure …", "the save it opens from is ready").
- Tests:
  - New: `run_v60_open_existing.js` (someone else's save, own save after saving, case-insensitive name, other person's draft only, brand new, unreadable folder, no View rework).
  - Updated for the new behaviour: `run_v54_check_measure.js`, `run_layers_discoverability_fix.js`, `run_v53_dialog_speed.js`, `run_viewer_fixes_and_rework.js`.
  - The full Site Measure/Viewer suite passes.

**v59 (2026-09-28):** Andrew, after field testing: *"Ok field tested and pretty good. Site measure app need to popup a small number pad when typing a measure. This to also have quick text like ctr, ftc, bhead, oall, text to be reduced to 18 and line weight to 2 as default."*

- **Measure pad.** Typing a dimension or angle label pops up a small on-screen pad (about 300 × 210 px) instead of the tablet's keyboard: 0–9, point, Space, ⌫, and the quick texts **ctr / ftc / bhead / oall**, plus **ABC**, which hands over to the full keyboard for anything else, and **Done**.
  - Quick text goes on with a space ("2400 ftc"). When the whole label is still selected, it's added on the end rather than replacing the number.
  - Keys act on press and never take focus from the label, so the caret and selection behave like a keyboard. A tap on the pad never reaches the plan.
  - The pad sits bottom-right, or whichever corner doesn't cover the label being typed. Tapping the plan still finishes editing.
  - The label is set to `inputMode "none"` so the tablet keyboard stays down; a real keyboard (PC) still types straight in.
  - Text and callout labels keep the normal keyboard.
- **Defaults: text 18, line weight 2** (were 32 / 4). A plan or check measure saved with the old untouched 32 / 4 moves to 18 / 2 once (`style.sizeDefaultsV59`). A size picked on purpose is kept, and marks already drawn keep their own size.
- Viewer: same build (it never edits, so it has no pad).
- Tests: new `run_v59_measure_pad.js` (real touch taps). `gen_test.py` gained a `makeAngle` hook. All 140 Site Measure/Viewer tests pass.

**v58 (2026-09-27):** Hides the **Schedule Backups** folder from the project list. Scheduler v29 now keeps its daily spreadsheet backups in that folder, directly in the main Projects folder (Andrew: *"a schedule backups folder directly in the main folder ... I meant in the main folder. Not the individual projects folder."*). Every app lists every folder in the main folder as a project, so each one now leaves that folder out: `isReservedRootFolderName`, the same one-line rule in every app. Tested across all 11 apps by `pdftest-projects/run_schedule_backups_folder_hidden.js`, which fails on every app's previous build and passes on the new ones.

**v57 (2026-09-27, same night):** Andrew sent the v56 **⏱ Timings** from the tablet (Level 1 · MH.028 · J.T.922). The page was shown after **154 s**: 84.8 s to list the item's layers and read his draft, then 69.4 s to read his own last save. Once the page was up, it read and drew the 3 other layers in **1.2 s**. So neither storage speed nor drawing was the problem. The open was **waiting in the storage queue** behind the background status scan that runs on every level open.

- That scan (`readJoineryStatuses`) handled every item in the project at once. For each one it walked Project Saves → Joinery Status → item (3 lookups), listed the folder, and re-read every event file. On a real project that's thousands of calls, and Android's storage layer runs them one at a time. Simulated at 20 ms per call with 150 items: the v55/v56 open waited behind about 1,100 calls (**21.9 s**). v57 opens in **0.5 s**. The cost grows with project history, which is how a build that was fine one night was slow the next.
- Fix:
  1. Item folder handles now come straight from the one Joinery Status listing, with no lookups.
  2. Event files never change once written (every status change is a new file), so each one is read once and remembered on the device in IndexedDB (`joineryStatusEvents\0<project>`). A repeat scan costs one listing per item: about 200 calls instead of about 1,100 in the same simulation.
  3. Items go through `runBackgroundQueue` at 3 at a time. It starts nothing new while the foreground is busy: from `fgBegin`/`fgEnd`, around opening a check measure, loading its other layers, and loading the job notes list. If the foreground somehow never ends, the scan carries on after 30 s. `listShopDrawingKeys` uses the same queue.
- The v56 double-tap guard, incremental layers and Timings panel all stay. Hidden layer groups are stripped from exports.
- Tests: new `run_v57_scan_priority.js` covers the queue limit, pausing for the foreground, fail semantics, the event cache (no re-read, new files picked up) and the pause during a real open. Diagnostics `diag_scan_contention.js` and `diag_open_item_photos.js` were added. The whole Site Measure/Viewer suite passes; the heavy PDF tests pass when run alone.

**v56 (2026-09-27, same day):** Andrew: *"open check measure still took upto a minute to open on tablet. this was super fast lastnight"*.

- Simulated Android storage (every folder/file call queued one at a time, 40 ms each, CPU throttled 6×) shows this build and last night's v49 making the same storage calls and opening in the same time, so storage isn't the regression. The CPU-side suspect: other people's layers were torn down and redrawn from scratch, with every photo decoded again, each time one more layer arrived (v54's background loading made that N× per open) and on every plan redraw while you drew. On a 4 GB tablet with several photo layers, that's the kind of cost that turns seconds into a minute.
- Fix: each layer's drawing is now built once and kept (`renderOverlayLayers` is incremental; groups are tagged `data-layer-idx`). A new layer appends just itself, and hide/show only toggles it. Everything is cleared when you leave the item.
- New **⏱ Timings** button on the Projects screen shows the last 10 Open check measures on this device, step by step: reading the item's layer list and your draft, the plan snapshot, save-before-leaving, reading your last save, page shown, and other people's layers drawn. It's kept on this device only, so if it's still slow on the tablet a screenshot pins it to the exact step.
- Tests: `run_v54_check_measure.js` gained v56 checks (existing layer drawings survive a new layer plus redraws, hide/show reuses the drawing, nothing is left drawn on the plan, and timings are recorded and listed). `gen_test.py`'s visible-object count ignores hidden layer groups. Site Measure/Viewer suite passes. (Three Install/Delivery ITP tests in the same folder time out on the new PIN sign-off flow. They test those apps, not this build, and are stale; fix them with those apps' next update.)

**v55 (2026-09-27, same day):** Andrew, urgent: *"site measure and viewer still gets a second popup when pressing open check measure, it should go straight to the check measure. as it did before. pressing open job note takes ages also to bring up the job note selector need to remove the delete option from long press on the site measure app (project / level / room etc)"* (and: fine on Windows, the problem is the tablet).

- **Straight to the check measure.** The long-press "Open/Add check measure" row and a double-tap on a marker now open the item's page directly — no in-between Joinery Item popup. Its two extra buttons moved onto the long-press menu: **View rework** (red, only when the item has a rework file) and, for a legacy project only, **Open this room's own plan**. In the Viewer the check-measure row is left off when nobody has saved one for that item yet (it would only open an empty page) — the same rule the old popup applied; the Viewer now uses the same background check-measure listing as Site Measure to know that.
- **Job notes open faster.** The "Project Saves/Job Notes" folder handle is remembered per project (one lookup + one listing instead of three lookups + a listing), and the list starts loading the moment the long-press menu shows a "View job note" row, so it's usually ready by the time it's tapped. (If the slow part is Android's own "open with" app picker after tapping Open on a note, that's the tablet, not the app.)
- **No delete on long-press in Site Measure.** Press-and-hold on a project, level or room row does nothing now (row tooltip is just "Tap to open"). The Viewer's own long-press is unchanged — it only hides an entry from that device's list, never deletes.
- Tests: `run_v54_check_measure.js` gained v55 checks (long-press row goes straight to the page; no delete on a project row). Updated for the new behaviour: `run_joinery_item_dialog.js`, `run_layers_discoverability_fix.js`, `run_flat_structure_interop.js`, `run_lock_popover_restyle.js`, `run_viewer_room_marker_popover.js`, `run_rooms_gate_and_delete.js` (now asserts a held room row offers no delete), `run_describe_error_fix.js` (delete-flow half removed). Retired (the feature they tested is gone): `run_delete_flow.js`, `run_delete_noop_bug.js`, `run_delete_timeout.js`, `run_removeentry_deep.js` → `retired_v55_*.js`. All 82 Site Measure/Viewer tests pass.

**v54 (2026-09-27, same day):** Andrew, on the Android apps: *"when you select open check measure on the plan, it opens another pop up with sub orders / close / open check measure on it, when you press open check measure it is very slow. it also shows sub orders if there is none for that joinery item. on site measure app, pressing open check measure on the second popup does nothing. remove sub orders from these 2 apps"* — then *"in site measure app, if there is no check measure yet, the button should say add check measure"*.

- **Sub orders removed from Site Measure and the Viewer** (the v53 button, dialog, CSS, write path and test hooks). Sub orders still live in UTZLINE Sub Orders, the ITP apps, Projects, Scheduler, Machine Schedule and Solid Surface Schedule.
- **"Open check measure does nothing" (Site Measure).** Every open rendered a snapshot of the whole plan first (the v46 first-open crop feature): the full plan SVG, every tile, serialised and then base64'd three more times over. On a big plan that could throw outright (string too long) synchronously inside the tap, so nothing happened; on a 4 GB tablet it was slow even when it worked, and it was thrown away whenever the item already had a check measure. Now the item's overlays and your draft are checked first (reads the page needed anyway, reused rather than repeated), and the snapshot is only made when you have nothing saved for that item yet. When it is made, only the plan tiles actually in view are included, it's loaded from a Blob instead of a giant base64 string, it times out after 20 s, and any failure falls back to a blank page instead of a dead button.
- **"Very slow" (both apps).** Opening a check measure used to wait for every other person's saved layer to be read in full (each a JSON file with its photos as base64, and every Save is its own layer). The page now opens as soon as the base view is read; the other layers are read one at a time afterwards and drawn as they arrive, still all visible by default. A short "Opening check measure…" toast shows straight away on a slow tablet.
- **"Add check measure" (Site Measure).** The long-press row and the Joinery Item dialog button say "Add check measure" when nobody has saved a check measure for that item yet, "Open check measure" once one exists. Known from one background listing of `Project Saves/Site Measures` alongside the status scan (3-minute freshness, IndexedDB snapshot) — no extra file lookup per tap — and updated the moment you save. Until the first scan lands it shows the old "Open check measure". The Viewer is unchanged (it already hides the button when there's nothing to open).
- Tests: new `pdftest-projects/run_v54_check_measure.js` (no Sub orders anywhere in either app; Add/Open labels; opening an already-saved item does zero plan serialisation and no crop dialog while a new item still gets it; other layers arrive after the page opens). `run_sub_orders_received.js` retired with the feature. The shared test harness (`redline_projects_test.html`, via `gen_test.py`) was regenerated from current source — it had been frozen at v47 — and four tests updated for changes since then (the Add/Open label, the v49 🏭 icon revert, the v50 modal popover). All 90 Site Measure/Viewer tests pass (the two heavy PDF-tile tests pass run on their own).

**v53 review fix (same build):** the Sub orders "mark as received" write is now strict per the family's "unreadable is not empty" rule — a mid-sync Orders file is retried once and then left untouched ("couldn't read (still syncing?) — nothing was changed"), an order unattached meanwhile writes nothing, and nothing is ever created. It writes the same pretty-printed JSON shape Sub Orders itself writes, and the Orders filename now uses Sub Orders' own `"file"` fallback for a blank name (this app's `sanitizeFileBase` falls back to `"plan"`). Three new failure-case checks in `run_sub_orders_received.js`, run against both apps.

**v53 speed fix (same build):** Andrew, the same day v52 shipped: *"site measure app is really slow again, we fixed it yesterday but something you did today has brought back all the slow downs."* Diffed every shipped build since v49 line by line: none of v46's speed fixes were lost, and v50 (popover restyle) and v51 (company logo, gate screen only) add no per-interaction file work. v52's "View rework" gate was the one piece of per-tap file I/O added today — every joinery item dialog open walked Install ITP's rework folder with uncached lookups (4 serialized storage calls per open on a legacy project, measured), and on the common project with no rework data every one of them a *miss*, the slowest kind of lookup on Android's storage layer — the exact cost v46 removed everywhere else. Fixed the same way v46 gated "View shop drawing": one background listing of which items have a rework file at all (`listReworkFileKeys`), refreshed alongside the status scan (3-minute freshness, IndexedDB snapshot for an instant first paint); a dialog open now reads nothing unless that specific item has a rework file (0 storage calls, measured — was 4). Before the first scan lands it fails open exactly as v52 did, so a real rework is never hidden. Viewer only: the "Open check measure" gate's per-open overlays-folder listing is now remembered per item for the same 3 minutes. New `pdftest-projects/run_v53_dialog_speed.js` (both apps) counts storage calls per dialog open for legacy, flat and no-rework-folder projects, and confirms items with a real open rework still show and list it.

**v53 (2026-09-27, same day):** "Sub orders" summary + write-back on the joinery item dialog. Andrew, verbatim, as a direct follow-up to today's earlier "Ordered" summary on the Scheduler app: "ok now we need all joinery summary pages to show the associated orders. with the option to mark them as recieved. the main schedule also needs a mark as received button for orders. on the schedule." This is this app's own slice (the Scheduler's own schedule-table button is a separate build).

A new "Sub orders" button on the joinery item dialog (`#joineryItemViewSubOrdersBtn`, always shown — unlike "View rework", not gated on whether any orders exist yet) opens a list dialog (`#subOrdersViewBackdrop`) of every order the standalone **UTZLINE Sub Orders** app has attached to that item, read straight from its own `Project Saves/UTZLINE Sub Orders/Orders/<Level> - <Room> - <Code>.json` (mirrors `redline-utzline-projects-pwa`'s own read-only v27/v28 card — see that app's own README for the data shape). Orders are grouped under one heading per type — the four base types first (steel/upholstery/timber/aluminium, each a fixed, colour-validated chip), then any custom type (Sub Orders v4+) alphabetically after, shown with its own real `typeLabel` and a neutral `.so-type-custom` chip rather than a guessed-at colour — each row shows the required-by date, supplier/PO/notes, and an Open button resolving straight to the order's own file under Sub Orders' `Files/` folder.

**New for this app family: a real write path.** Each order row has a "Received" checkbox + date input — ticking it (auto-filling today's date if empty), unticking it, or changing the date while ticked all write straight back into the same `Orders/*.json` file, read-modify-write, same interaction Sub Orders' own "View orders" list already uses. The write is a **shallow copy** of the existing record (`Object.assign({}, o, {received, receivedDate})`), never an explicit field list — Sub Orders' own `setOrderReceived` hit exactly this bug today (shipped its own v5 fix the same day) when an allowlist predating the `typeLabel` field silently dropped it on every write; this app's write path is built the same shallow-copy way from the start for the same reason.

**Viewer write-access decision:** the Viewer gets the exact same read+write access here as this app. Andrew's request said "ALL joinery summary pages," with no Viewer carve-out, and marking an order received is a procurement/status update, not a measurement edit — the same class of exception "Add job note" already established (a narrowly-scoped, requested-only-at-write-time `readwrite` permission upgrade on the active project's own folder, via the existing `ensureProjectWritePermission()`), reused here rather than inventing a second gate. Every OTHER write in the shared `source.html` stays exactly as Viewer-blocked as it always was.

New `pdftest-projects/run_sub_orders_received.js` covers: grouping by type; a custom type's real `typeLabel` with the neutral chip; the empty state; the received checkbox/date write-through to the real `Orders/*.json` file, confirming the shallow-copy write preserves every other field on the record it touches (including `typeLabel` and a made-up future field) rather than trimming to an allowlist; and that this app never touches Sub Orders' own `Inbox/` folder (confirmed byte-for-byte unchanged) — all run against both real production bundles (this app + Viewer). Full pre-existing regression suite re-run (136 pass; the only 4 failures were pre-existing, environment-load-related, and in completely unrelated apps — `redline-itp-pwa`/`redline-delivery-itp-pwa` ITP signoff tests untouched by this change — confirmed by rerunning each in isolation).

`service-worker.js` cache → `utzline-sitemeasure-cache-v53`.

**v52 (2026-09-27, same day):** "Viewer fixes" queue item (`NEXT_RUN_NOTES.md`, dictated 2026-09-27) — five related changes to the shared joinery-item-page/overlay-layers machinery in `source.html`, built and shipped together:

1. **All check-measure overlay layers already default to visible** when a joinery item's page is opened. Investigated before writing anything: every per-person saved layer gets `visible: true` unconditionally at construction (`openJoineryItemPage`'s own `otherLayersPromise`) — there's no stored on/off state anywhere to have regressed. No code change; added a regression assertion (`run_viewer_fixes_and_rework.js`) against the real construction path so this stays true.
2. **"Open check measure" button removed when nothing's been saved yet for an item — Viewer only.** Andrew, verbatim: "if there has not been a check measure for a joinery item, remove the open check measure button." Applied only under `VIEW_ONLY_MODE`: Site Measure (this app) still needs the button unconditionally, since it's the only way to start an item's very first check measure. The Viewer checks `listJoineryOverlays()` before showing the button, hiding it (rather than showing-then-hiding) until at least one saved overlay is confirmed.
3. **New "View rework" feature (read-only).** Andrew, verbatim: "add a view rework button if a joinery item has a rework, make the text red on this button. also add this to the main menu for a project." Neither app ever read Install ITP's rework data before. A red-text button (`#joineryItemViewReworkBtn`, new `.mbtn-rework` class + a new `--danger` CSS token — deliberately a different, more saturated red than this app's own `--accent`, which is already red/orange) appears on a joinery item's dialog only when that item has at least one OPEN rework entry (Install ITP's own "outstanding" definition: `!entry.closed`). A matching project-level "View rework" menu entry (`#viewProjectReworkBtn`, next to "Export all PDFs (whole project)") lists every open rework anywhere in the project. Both open the same simple read-only list dialog (`#reworkViewBackdrop`) — cabinet number, detail text, who/when logged, and (for the project-wide list) the level/room/code it belongs to. Reads Install ITP's own files directly and never writes them: flat projects at `Project Saves/UTZLINE ITP/Install ITP Rework/<Level> - <Room> - <Code>.json`, legacy projects at `itp-install-rework/<Level>/<Room>/<Code>.json` (exact same paths/filenames Install ITP itself uses).
4. **Layers panels no longer overlap.** The "Site Measure layers" panel (`#overlayLayersPanel`, foreign check-measure overlays) now docks LEFT; the "Objects/Layers" panel (`#layersPanel`) stays docked RIGHT — both can be open together now without covering each other.
5. **"Faded images" regression fixed.** Andrew: "when opening a site measure, some images are coming out faded. we already fixed this once." Root cause: a foreign layer's own photo opacity (0.55) was compounding with the wrapping layer-group's OWN opacity multiplier (0.75, ≈0.41 effective) on top of a grayscale filter — especially noticeable now that (per item 1) every foreign layer already shows by default. Per-image opacity is now 0.7 and the redundant group-level multiplier is removed entirely.

New `pdftest-projects/run_viewer_fixes_and_rework.js` covers all five against both real production bundles (this app + Viewer) — including both a legacy-shaped and a flat-shaped fake project for the rework paths. Full pre-existing regression suite for both apps re-run clean (9+ files, plus every joinery-item/layers/overlay/rework-related test individually), no page errors.

`service-worker.js` cache → `utzline-sitemeasure-cache-v52`.

**v51 (2026-09-27):** Read-only "Company logo" preview (NEXT_RUN_NOTES.md item 8's family-wide scope, confirmed 2026-09-27: "every other app" means ALL apps, not just the three ITP apps already fixed as Install ITP v40/Manufacture ITP v19/Delivery ITP v19). Andrew, verbatim: "change company logo should only be visable in the projects app, in every other app it should load the one chosen in projects." This app never showed a company logo anywhere before now — a small "Company logo" card was added to the top of the Projects screen (`#gateList`), right below the folder label and above the project list: a 56×56 preview box (or a "No logo" placeholder) plus a one-line "set in the UTZLINE Projects app" caption, read-only, no upload/remove controls of any kind. Sourced from the exact same shared `company-logo.png` file UTZLINE Projects itself owns at the Projects root (read via `projectsRootHandle`, the same root `utzline-users.csv` already comes from) — `readCompanyLogoReadOnly()` mirrors Projects' own `readCompanyLogo()` (an object URL from the raw file bytes, not a downscale-to-data-URL copy, since this app has no PDF export of a logo to feed). Refreshed via `refreshCompanyLogoPreview()`, called from `showGateState()` alongside the existing `populateIdentitySelector()` call — so it's kept current every time the gate is (re)shown (boot, choosing/reconnecting/switching a Projects folder, "All projects", etc.), best-effort with no error if the file simply isn't there yet. New CSS (`.company-logo-row`/`.company-logo-preview`, matching UTZLINE Projects' own `.logo-row`/`.logo-preview` look) added to `source.html`. New `pdftest-projects/run_company_logo_readonly.js` (no upload/remove UI anywhere in the DOM; the preview shows/hides correctly with/without `company-logo.png` at the root, no error either way, in BOTH this app and the Viewer — built from the same source). Full pre-existing regression suite for both apps re-run clean (9 files).

`service-worker.js` cache → `utzline-sitemeasure-cache-v51`.

**v50 (2026-09-26, same day):** `NEXT_RUN_NOTES.md` item 1 — the right-click/long-press context menu (`showLockPopover`) restyled to match Install ITP's own `#markerMenuBackdrop` pattern. Andrew, verbatim: "make the site measure and viewer long press menu options look like the install itp long press menu options."

- The old small floating icon-row popup, anchored right at the click point, is now a centered modal with a dimmed backdrop and full-width pill-shaped rows — applies generically to every object type's popover (roomlink, text, callout, dimension, angle, image, base site photo), not just one. A new title line shows context: the joinery code for a roomlink, a plain type label ("Text"/"Callout"/"Measurement"/"Angle"/"Image") for other object types, "Site photo" for the base image. The single most relevant action (the first context row built) gets the same accent styling as this app's other primary buttons (`.mbtn-accent`, reusing the app's own existing accent color rather than introducing Install ITP's literal green — this ports the LOOK/pattern, not a hardcoded hex); every other row stays a plain neutral pill. There's no rework-style destructive row left in this popover any more (removed 2026-09-24 along with "Delete on right click"), so no red/danger variant was needed. A new "Cancel" row is always last, and clicking the dimmed backdrop itself (not the box or a row) also closes the menu, matching Install ITP's own dual dismiss behavior.
- Class names (`.lock-popover-wrap`, `.lock-popover`) are deliberately UNCHANGED from the old floating-popup version — only their CSS changed, and `showLockPopover`'s own row-building logic (`popoverRow()`, the row list per object type/gating) is untouched — so every existing regression test that finds a row via `.lock-popover`/reads its label via `.lock-popover span` keeps working unmodified.
- Also added: a small, deliberately minimal `window.__testHooks` (`ready`, `showLockPopover`, `hideLockPopover`, `isViewOnlyMode`) — this app's live built `index.html` had no test-hook surface reachable from outside its own closure at all (the only existing regression coverage for this app runs against `pdftest-projects/redline_projects_test.html`, a frozen fixture many versions behind the current app, with its own much larger, incompatible `window.__test` hook object from an earlier architecture — out of scope to refresh as part of this change). New regression test `run_lock_popover_restyle.js` (in `pdftest-projects`) drives this new hook directly against the current built app for both Site Measure and Viewer, covering the backdrop/box/title/accent/Cancel/backdrop-click-to-close behavior across roomlink, dimension, and base-photo cases. The 7 existing tests that already load the live built app (`prod_smoke_v24.js`, `smoke_v29/v31/v32.js`, `smoke_v34_projectinfo_e2e.js`, both `toolbar_visual_check*.js`) re-run clean.
- Rebuilt via `build.py` from `source.html` (shared canonical source with the Viewer, see that app's own README entry). `service-worker.js` cache bumped to `utzline-sitemeasure-cache-v50`.

**v49 (2026-09-26, same day):** Family-wide status icon revert (`NEXT_RUN_NOTES.md` item 2, schema §4) — this app's own portion. A full revert of both statuses to what they were before the earlier icon-sweep round touched them: `machined`: 🪚 → ⚙️; `in_manufacture`: 🔨 → 🏭. Both `joineryStatusIcon` and `joineryDisplayIcon` (the plan-marker icon, a second hardcoded copy of the same cases) updated in `source.html`, rebuilt into this app via `build.py`. Grepped for every literal 🪚/🔨 occurrence (not just those two functions) — no other hits (no banner text like Manufacture ITP's). UTZLINE Projects already shipped this in its own v25; Viewer, Install ITP, Manufacture ITP, Delivery ITP, Scheduler, Machine Schedule, and Solid Surface Schedule each ship it on their own next update, per the standing per-app process note. `service-worker.js` cache bumped to `utzline-sitemeasure-cache-v49`.

**v48 (2026-09-26):** Andrew's dictated punch-list, verbatim: "change the
open joinery item on right click button to say open check measure" /
"remove the bring to front button" / "in the individual check measure
page, when you click save and exit, have a popup come up that asks is
this check measure complete. if yes it will mark the item as check
measured" / "remove the ability to move the indicators (circles /
icons)" / "remove the unlock button on indicators, only images and
dimensions, callouts etc should be movable. lock all on save / save and
exit."

- **"Open joinery item" → "Open check measure".** Both places this shows up
  — the right-click/long-press popover's own row on a roomlink marker, and
  the matching accent button on the Joinery Item dialog it opens — relabeled.
  Same destination (`openJoineryItemPage`), wording only.
- **"Bring to front" removed.** The popover row (offered for every real,
  unlocked object type since the v30 layering fix) is gone entirely, along
  with its now-unused icon. "Send to back" (image-only) is untouched — not
  mentioned, and it's a separate action. There is no longer any in-app way
  to reorder an object back above whatever is currently covering it.
- **"Is this check measure complete?" on Save & exit.** Clicking Save &
  exit on a joinery item's own page now asks this first (the same generic
  yes/no dialog every other confirm in this app already uses). Answering
  yes marks the item "measured" in the shared `joinery-status.json` once
  the save actually lands — the same forward-only write the popover's own
  "Mark as check measured" row already makes, so it's always safe even if
  the item is already further along (manufactured/installed). Answering no
  just saves & exits as before, with no status change.
- **Indicators (roomlink markers) can no longer be moved.** A plain tap
  still selects one (so the properties panel/delete still work), but the
  drag that would reposition it never starts, regardless of its own
  `.locked` flag. No Lock/Unlock control is offered for one any more either
  — not in the right-click popover, not in the layers panel — since there's
  no longer a "locked" state for one to opt into or out of. "Lock all" and
  the auto-lock-on-exit sweep both leave indicators alone now (they were
  never truly draggable to begin with, and are no longer part of that
  bookkeeping).
- `service-worker.js` cache bumped to `utzline-sitemeasure-cache-v48`.

**v47 (2026-09-26):** Status icon change — Andrew, verbatim: "change in
manufacture to this 🔨 and machined to this 🪚." `joineryStatusIcon` and
the plan-marker `joineryDisplayIcon` both updated (`in_manufacture`: 🏭 →
🔨; `machined`: ⚙️ → 🪚); no other status icon changed. `service-worker.js`
cache bumped to `utzline-sitemeasure-cache-v47`.

**v46 (2026-09-25):** Andrew's "full check of the site measure app" round, plus
two follow-ups sent while it was in progress. Verbatim: "See how speed can be
improved. Fix the top menu bars that dont work on android properly. Fix the
device back button closing the app. Icon caching like we just did. When
returning to plan it goes back to the last view. When opening joinery item
for the very first time make it import a snapshot of the current screen
that's croppable But without the indicator icons or indicator text on it so
we can write measures straight onto that snapshot. Also sometimes when a
snapshot is imported the crop markers can't be adjusted across the entire
import" / "And only show view shop drawing or view job notes button if there
is one applied also lock all on save and exit" / asked what the toolbar did
wrong on the tablet: "Wrong function. Back goes to new page as does home."

- **Back/Home → "new page" (the toolbar report).** Root cause found in the
  level-file read: `readFlatLevelFileByName` returned `null` for ANY failure,
  and `openFlatLevel` took `null` to mean "brand-new level" → `startBlankPlan`.
  A level file that exists but momentarily can't be read/parsed on the tablet
  (Dropbox still syncing it in, a truncated SAF read) therefore reopened as a
  blank white sheet — exactly "Back goes to a new page" — and its blank
  autosave could then have been written over the real plan. Now: missing and
  failed reads are told apart (`{code:"level_read_failed"}`), a failed read is
  retried once, and if it still fails the app lands on the level list with a
  clear "may still be syncing — try again" toast, never a blank canvas; the
  legacy (folder) shape gets the same rule; `saveFlatLevelFile` refuses to
  write a blank canvas over a level whose file holds a real plan
  (`blank_over_real_level`). Home also always goes to the project list
  whenever any project context exists (the "start a new file" branch is now
  strictly the no-Projects-folder single-file mode).
- **Return to plan = the last view, instantly.** Leaving the level plan for an
  item (or a legacy room) remembers exactly what was on screen — base image,
  every object incl. markers, and the pan/zoom (`rememberLevelPlanForReturn`,
  keyed project+level, 10-minute freshness). Back reuses it with no file read
  at all (`openLevel(name, {preferRemembered:true})`) and `fitOrRestoreLevelView`
  puts the view back where it was; a plain re-open from the level list still
  reads the file fresh but also restores the remembered view.
- **Device Back button walks back through the app** (same design as Install
  ITP v35): `navRecord` keeps one history entry per screen — setup/reconnect/
  project list replace (base), levels → level → room/item push. `popstate`
  first closes whatever dialog/panel/popover is open (`closeAnyOpenOverlay`),
  otherwise steps back one screen via the existing navigations (item → plan,
  room → plan, plan → level list, rooms list → level list, level list →
  project list), honouring the unsaved-changes confirm (cancel puts the entry
  back). NB `history` inside source.html is the undo stack, so this uses
  `window.history` explicitly. The Exit button unwinds its own entries before
  `window.close()`. `switchProject`/`goHome` stay fire-and-forget; the Back
  handler uses new `switchProjectP`/`goHomeP` to know when they settle.
- **Icon (status badge) caching.** `joineryStatusCache` is kept for the whole
  project across level opens and Back, re-scanned in the background only when
  older than 3 minutes (a fresh open from the level list always re-scans in
  the background; badges paint from the cache first); a device-local
  IndexedDB snapshot (`joineryStatusSnapshot\0<project>`) paints them
  instantly on a project's first level; `joineryStatusIndex` makes the
  per-marker lookup a hash lookup instead of a linear scan per render.
- **First-open snapshot for a joinery item.** `renderPlanViewSnapshotForItem`
  renders what is in the viewport (clipped to the plan) at real on-screen
  density (1600–2600 px long edge, JPEG 0.9) with every roomlink marker — dot,
  code label, status badge — and any other-person overlay layer stripped, no
  title block. On an item's very first open (nothing saved by anyone) it is
  offered in the crop dialog ("Crop the plan snapshot for … / Use full view")
  and the result becomes the page's base image; the page is left clean until
  something is drawn (no save prompt / draft for an untouched page). Editor
  only; the Viewer is unchanged. Test harness default is the old blank page
  (`setFirstOpenSnapshotModeForTest`).
- **Crop handles couldn't reach the whole image:** `renderCropRect` positioned
  the rectangle from the stage's top-left while `cropRect` is measured from the
  displayed image's top-left; any image narrower/shorter than the stage (a
  portrait photo capped by max-height and centred) drew the rect offset, so the
  clamps stopped it short on one side. The image's own offset is added, the
  stage has 12px padding so an edge handle isn't clipped, and a resize
  re-clamps.
- **View shop drawing / View job note only when one exists:** the popover's
  "View shop drawing" row is gated on `shopDrawingKeySet` (item keys with at
  least one drawing folder, refreshed with the status scan and snapshotted
  with it); unknown-yet reads as "show". "View job note" was already gated.
- **Save & exit + lock all:** on a joinery item page the Save button reads
  "Save & exit": every object is locked (the v31 auto-lock rule), the overlay
  + PDF are written, and it returns to the level plan at the remembered view.
  A mid-work Save on a level's own plan still stays put and locks nothing
  (deliberate, unchanged — see run_multi_room_save.js).
- **Speed:** one reused IndexedDB connection per database (the same leak fixed
  in Install ITP v35); no per-edit IndexedDB "current" copy while a project
  level/room/item is open (only the single-file fallback reads it); drag
  re-renders coalesced to one per animation frame (`requestRenderAll`); other
  people's overlay layers re-rendered only when the visible-layer set changes;
  `listFlatLevelNames` stats files in parallel and caches name↔size/mtime per
  project in IndexedDB instead of reading every level's full JSON to list
  names; rolling backup PNGs capped at 6000 px (`BACKUP_PNG_MAX_DIM`) instead
  of up to 16000; a plain tap on an object (a marker) no longer counts as an
  edit (no phantom "Save changes?" prompts, no needless level rewrites).
- **Real bug found on the way:** every `<img>`-based SVG rasterisation
  (rolling PNG backups, Share "current view", the PNG fallback) had been
  failing since the `#world` markup gained comments containing "--" — legal
  HTML, not legal inside an XML comment, so the serialised SVG was malformed.
  `cloneWorldForExport` now strips comment nodes. Four suite tests that had
  been failing in the sandbox for this reason pass again.
- Also: double-tap on a marker falls back to a hit-test at the tap position
  when the first tap's re-render detached the tapped element; toolbar rows
  declare `touch-action:pan-x` and their controls `manipulation`.

Tests: new `pdftest-projects/run_v46_site_measure_round.js` (17 checks: level
read failure never blanks, remembered plan + view, history Back through
dialogs/item/plan/levels/list, first-open snapshot has no marker + crop dialog
wording + crop rect offset + crop applied + clean, Save & exit locks/saves/
returns, one IDB connection, tap not dirty, blank-over-real save guard);
`run_shop_drawings.js` / `run_joinery_status_and_job_notes.js` updated for the
gated View row; full Site Measure suite green apart from the two pre-existing
failures that target other apps' harness. `service-worker.js` cache →
`utzline-sitemeasure-cache-v46`.

**v45.14 (2026-09-24):** Two fixes from the same round, Andrew, sending two
screenshots (a Joinery Item page and a plan view with a pink/magenta tint)
together with a large multi-part request; these are the two parts of it
that land in this app. First: "get rid of the horrible light red background
hue." Root cause was `renderOverlayLayers()`'s per-layer CSS `filter:
sepia(1) saturate(6) hue-rotate(Ndeg)`, meant to color-code each other
person's saved reference layer — every stroked shape/label here draws a
three-pass white-ring/black-ring/color halo (see `haloOuterW`'s own
comment) for legibility on any background, and that filter chain turns
white into a highly saturated color BEFORE hue-rotating it (plain
hue-rotate alone leaves true white/black untouched — it's the sepia() step
that injects the color). With the halo being the single largest painted
area on a busy plan, or a full-size reference photo, that read as a solid
magenta wash over the whole canvas rather than a subtle per-layer tint.
Fixed by dropping the filter entirely: a new `tintOverlayObjectGroup()`
walks each rendered reference object's own SVG elements directly, drops
every element that's purely the white/black halo (or a text/callout box's
white background, which rode along with the halo on the same element —
reference layers don't need any-background legibility the way your own
live drawing does), and recolors whatever's left to a flat, calm accent
color from a small palette (first slot still purple, per Andrew's own
"purples layers" naming). A photo layer gets a fixed, modest
grayscale+dim instead of any hue shift, since a raster image can't be
recolored element-by-element the way vector shapes can, and a flat dim
can never produce a solid colored block the way the old filter could.
Second: "the joinery status page needs finishing... it currently does not
show the site measure overlay. this should come from joinery item
(image)." A permanent overlay file has always stored its objects as
vector data — showing that faithfully needs this app's own `renderObject`,
which nothing outside Site Measure/Viewer has. Rather than have UTZLINE
Projects (and eventually Scheduler/Machine Schedule) reimplement a vector
renderer just to show a static picture, every explicit Save now also
flattens the same base photo + objects being saved into one ordinary
raster image (`renderJoineryOverlaySnapshot`, capped to a modest preview
size) and stores it right alongside the vector data, as
`overlaySnapshotDataURL`, in the same overlay JSON file — any app can then
just show it as a plain `<img>`. UTZLINE Projects' own "Site Measure
overlays" card (previously a placeholder) now reads and shows it — see
that app's own README. Covered by the existing full overlay-layers test
suite (all passing, confirming the tint fix didn't change WHICH layers
render, only how) plus a new `run_site_measure_overlay_card.js` in
`pdftest-projects/` for the snapshot capture. `service-worker.js` cache
bumped to `utzline-sitemeasure-cache-v45.14` (and the Viewer's own
`utzline-viewer-cache-v45.14`, since both share this source).

**v45.13 (2026-09-24):** joinery-status.json v2 — Andrew, verbatim, describing the scale this whole app family now needs to handle: "we will have 30 people using this app in different stages, all coming back to the same database. there would be for instance upto 5 different machinist, 5 site managers (installers) 5 delivery drivers, etc. needs to be foolproof and nevel lose data. some of this will be done via dropbox upload after the fact. it then needs to all tie together without deleting." The shared `joinery-status.json` (read/written by this app, the Viewer, and every ITP/Schedule app in the family) used to be one JSON array file, read-modified-and-rewritten-whole on every save — no lock, no version check, and with up to five writer apps and 15+ people all saving into the same file, some via a Dropbox sync that could land minutes or hours late, a genuine risk of one save silently overwriting another's history. Rebuilt as an event-sourced store: every status change or job-note signal now writes its own small immutable file under `Project Saves/Joinery Status/<Level> - <Room> - <Code>/`, named `<user> - <timestamp> - <kind>.json` — the current status is always computed by folding an item's own event files together, so two writers can never collide (they're never touching the same file) and nothing can ever be lost regardless of write order or how late a sync lands. The old file is migrated automatically, losslessly, and idempotently the first time any app in the family opens a project after this update (concurrent double-migration produces no duplicates), and is left on disk afterward, untouched. Every existing render/display call site across every app in the family is completely unchanged — `readJoineryStatuses`/`setJoineryStatusForward`/`markJobNoteAdded` still return and accept the exact same shapes as before. The Viewer's own read path is unaffected beyond the same migration. Covered by a new dedicated test, `repro_status_event_sourcing.js` (`pdftest-projects/`), plus every existing joinery-status test across the family updated to read the new event files. This is the first of several planned rounds — Andrew asked for the shared status pipeline first since it's used by the most apps; Scheduler's own schedule file, Machine Schedule's cut-flags file, and Solid Surface Schedule's file are next. `service-worker.js` cache bumped to `utzline-sitemeasure-cache-v45.13`.

**v45.12 (2026-09-24):** Andrew, immediately after v45.11 shipped, verbatim: "still not working, every site measure save needs to be added as an extra layer, using the username and datetime in the filename. this is to eliminate any risk of deletion. (layername selecter could be username - date) alll username layers need to be accessible regardless of who is using the viewer." v45.11 had fixed a real but different bug (two devices computing different storage *keys* for the same duplicate-coded item). This report is about the layer *list* itself: every explicit Save has, since v45, already written its own immutable `<user> - <datetime>.json` overlay file that's never overwritten or deleted (see the "Site Measure overlay layers" comment above `listJoineryOverlays` in `source.html`) — so nothing was ever actually at risk of deletion. The real bug was that `openJoineryItemPage()` collapsed that file list down to only each user's single *newest* overlay before ever handing it to the layers panel, silently dropping every earlier Save — someone's own older Saves included — from the toggleable layer list, even though the files themselves sat untouched on disk. With two people saving twice each, the old code could only ever surface two layers total instead of four; that's Andrew's "only gives you one name to exclude."

Fixed by keeping every single permanent overlay file as its own distinct, individually toggleable layer. Site Measure still treats your own most recent Save as the base, editable view (unchanged) — everything else, including your OWN older Saves, is now offered as a "username — date" toggle layer exactly as the already-correct `renderOverlayLayersPanel` display code has labelled things since v45 (that part never needed to change — only the list feeding it was too narrow). The Viewer, which has no "own" layer, still shows the single most recent overlay across everyone as its base view, with literally every other saved layer from every user and every timestamp now offered underneath it as a toggle, not just each other user's latest. New reproduction test `pdftest-projects/repro_all_saves_as_layers.js` proves a user saving the same item twice produces two separately-named, independently-toggleable layers for everyone else (and for the Viewer). Full suite re-run: 91/97 passing, the same 6 pre-existing/already-documented failures as v45.11's notes, none new.

**v45.11 (2026-09-24):** Andrew, verbatim: "check measure layering is still not working properly, on windows or app, only gives you one name to exclude, needs to show all names." Root cause: `resolveJoineryItemPageKey()` disambiguates two joinery markers that happen to share the same Room+Code by sorting every marker with that Room+Code *currently loaded on this device* — a real, known scenario (duplicate codes happen "by habit," flagged since v44.3). Since crews sync a level file via Dropbox at different times, two devices can easily hold different local copies of which duplicate-coded markers exist at the moment either one opens an item, so they compute *different* storage keys for what a person on the ground would call the same item — their Site Measure overlays (and job notes, and shop drawings) silently split across two folders and stop seeing each other's layers at all. Confirmed directly with a new reproduction test before touching any code.

Fixed by caching a colliding marker's disambiguated key permanently on the marker itself the first time it's resolved, and silently re-saving the level file right away — every device, in every future session, then reads the same already-decided key instead of re-guessing it from whatever else happens to be loaded locally. Scoped to ONLY markers actually involved in a real collision; an ordinary non-duplicate item (the overwhelming majority) is completely unaffected, no extra background write. A real implementation pitfall — the first draft of this fix raced `openJoineryItemPage`'s own state swap and briefly wiped a flat level's markers in testing — was caught by the existing regression suite before shipping and fixed with a dedicated, synchronous-capture-then-write helper. New tests: `repro_duplicate_key_divergence.js`, `repro_multi_layer_bug.js`. Full suite re-run: 91/97 passing, same 6 pre-existing/already-documented failures as before (4 sandbox flakes + 2 intentionally-broken UTZLINE Projects tests, both unrelated to this app), none new. See `next-version-notes.md` for the full write-up.

**v45.10 (2026-09-24):** Two independent requests from Andrew, sent together.

*Part 1 — four button changes, verbatim: "remove the following buttons from the site measure app. delete on right click. New button, change to home button. Open. Open Project. Add shop drawing on right click (Keep view shop drawing)."* (1) The right-click/long-press popover's "Delete" row is gone — deleting still works via the properties panel's own trash icon, a separate, untouched control; there's only ever been the one popover in the app (`showLockPopover`), used for every object type and joinery markers alike, so there was no second menu to disambiguate. (2) "New" is gone; a new "Home" button (`#homeBtn`) takes its place, wired to a brand-new `goHome()` function — auto-saves whatever's open, then always lands all the way back on the top-level project list (a real improvement over the old New/Switch project, which only ever returned to "this project's own level list"). No existing header/logo click-to-home function existed anywhere in the app to reuse (confirmed by grepping every click handler on the brand mark) — wiring the header logo itself to Home is still a separate, open item, see `next-version-notes.md`'s pending list. **Judgment calls, disclosed:** unlike the old New button, Home stays visible in the Viewer too (pure navigation, same class as Switch project/Rooms, both already viewer-visible); the old button's press-and-hold "clear this level in place" gesture was deliberately dropped, not carried over, since a hidden destructive action under a button now labelled "Home" is a hazard nobody asked to keep. (3) "Open" and "Open Project" — confirmed genuinely two separate buttons, not one with two labels — are both removed entirely, along with their hidden file inputs and the now-dead `openProjectFile()` function. Open's job (loading a photo/plan image or PDF as the base plan) is still fully covered by "Insert image" and by drag-drop/paste, both untouched; Open Project's job (re-opening a downloaded `.utzline.json` file in the plain single-file fallback mode) has no remaining "read it back" button, though `saveProject()`/`projectPayload()` — the writing half — are untouched. (4) The popover's "Add shop drawing" row is gone; "View shop drawing" is completely untouched, exactly as asked — the whole write-side (add dialog, revision picker, file handling) is gone with it, since nothing else in the app offered a non-right-click way to add one.

*Part 2 — the multi-person "Site Measure layers" panel (shipped v45.0), per Andrew's fresh report today: "we still dont have the layer button for different peoples check measure saves, this is a must."* Investigated in full before changing anything: re-running the existing end-to-end regression test (`run_site_measure_overlay_layers.js`, unchanged) confirms the underlying mechanism was never broken — explicit Save still creates a permanent, immutable, username+datetime-stamped overlay per joinery item, autosave still only touches a private draft, and the layers panel still correctly lists and toggles every other person's saved layer in both this app and the Viewer. The real problem was discoverability: the layers button is a bare icon, easy to confuse with the unrelated pre-existing "Objects" button right next to it, appearing/disappearing in an already-crowded toolbar with no visible label ever — its tooltip never shows at all on the touch tablets this app actually runs on. Fixed with two small, targeted additions (not the larger, separately-tracked "select an active joinery item before drawing" redesign, which stays out of scope): a new always-visible count badge on the button itself, and the item-opened toast now names the other layer(s) by count and points at the icon directly, every time there's at least one to see. Reproduced end to end with a new Playwright test, `pdftest-projects/run_layers_discoverability_fix.js`, that drives the real right-click → popover → dialog path (never calling the internal open function directly) for two different real people with two real, differently-timestamped Saves, with screenshots confirming a third person sees both offered with a badge reading "2" in this app, and the Viewer separately shows the most recent as its base view with the other still offered and a badge reading "1" — exactly as documented.

New/updated regression coverage: `run_layers_panel_no_delete.js`-style checks folded into existing shop-drawing/room-marker/viewer-mode tests updated for the removed rows/buttons (`run_room_markers.js`, `run_joinery_status_and_job_notes.js`, `run_viewer_mode.js`, `run_flush_before_picker.js`), `run_shop_drawings.js` rewritten for the add-side removal, a new `run_home_button.js` (replacing `run_new_button_repoint.js`), and the new `run_layers_discoverability_fix.js` above. Full suite re-run: 91/97 passing — 2 failures are `redline-utzline-projects-pwa` tests (a different app, out of scope for this change), and the remaining 4 are the same pre-existing, already-documented sandbox SVG-rasterization flakes as every prior release's notes (`run_dead_backup_picker_removed.js`, `run_flat_structure_interop.js`, `run_level_backup_snapshots.js`, `run_view_snapshot_quality.js`), independently re-confirmed unrelated to this change (isolated repro shows small and font-embedded SVGs rasterize fine in this sandbox; only this feature's full-resolution, up-to-16000px real-plan-sized export image fails — a resource/canvas-size ceiling of this specific container, not a code bug).

- Cache versions bumped: Site Measure → **v45.10** (`utzline-sitemeasure-cache-v45.10`), Viewer → **v45.10** (`utzline-viewer-cache-v45.10`).
- What this release deliberately does NOT do: it does not touch the still-open header/logo-click-to-home item (pending list item 4) or the still-open "select an active joinery item before drawing" workflow redesign — both remain exactly as open as before. It does not touch any other UTZLINE app.

**v45.9 (2026-09-23, same day):** Andrew's "manufacture status" pipeline splits again — a new "machined" stage (⚙️, rank 3) now sits between `"in_manufacture"` and `"manufactured"` in the shared, byte-for-byte-mirrored `joinery-status.json` rank table, written by a brand-new sibling app, **Machine Schedule**, built in parallel with this change. Every rank at or above the old `"manufactured"` (3) shifts up by one across the whole UTZLINE family: `manufactured` → 4, `delivered` → 5, `installed` → 6. `joineryStatusRank()`, `joineryStatusIcon()`, and `joineryDisplayIcon()` all got the new `"machined"` case (⚙️) and the renumbered ranks. This app remains a pure read-only consumer of `"machined"`/`"manufactured"`/`"delivered"`/`"installed"` — it still only ever writes `"measured"` (and, since v45.8, `"in_manufacture"` via job notes) itself — so no new write path was added; this is purely keeping the shared enum's rank/icon tables in sync across the family so this app's own on-plan status badge and rank comparisons stay correct. No status-filter dropdown or other place in this app lists status values explicitly, so nothing else needed updating.

**v45.8 (2026-09-23):** Andrew, verbatim: "when a joinery item gets a job note (not shop drawing) it should update the joinery status to in manufacture in the joinery register." Reverses (in a different direction) the same-day v45.2/v45.3-era decoupling that made a job note a purely independent 🛠️ flag — `addJobNote()` now ALSO advances the shared forward-only `joinery-status.json` pipeline to `"in_manufacture"` (🏭), same call every ITP app already uses (`setJoineryStatusForward`), so an item already at `manufactured`/`delivered`/`installed` is never pushed backward by a job note added after the fact. The independent `jobNote`/`jobNoteAt`/`jobNoteBy` flag is unchanged and still tracked separately (it's what gates "View job note"'s own visibility) — this just also writes the pipeline stage now. Shop Drawings remain completely unwired from the pipeline, exactly as before.

Also, separately: Andrew, verbatim, on browsing the Job Notes folder in a file manager: "dont want job notes to have this format at the start on the filename 2026-09-23 23-17-48." A job note's filename now reads `"<original name> - 2026-09-23 23-17-48.pdf"` — the timestamp moved from the front to the end — rather than dropping it outright, since it's what keeps notes sorting newest-first (same "filename is the source of truth for when" convention UTZLINE Data Standard v1 §5 already uses for overlays). `listJobNotes()`'s own sort now extracts the stamp from wherever it sits in the name, so a job note saved before this change (still in the old prefix-first format on disk, never renamed) keeps sorting correctly alongside new ones. UTZLINE Projects' own read-only `listJobNotesForItem()` got the identical sort-key fix, so its "View job note" list stays consistent with this app's.

New regression coverage added directly to `run_joinery_status_and_job_notes.js` (in `pdftest-projects/`): the in_manufacture-on-job-note wiring (badge shows 🏭, not 🛠️, and the underlying record's status is `in_manufacture`), the new suffix-timestamp filename shape, and a mixed old-prefix/new-suffix sort-order check confirming both formats still sort strictly newest-first together. Full suite re-run: 82/86 passing, the same 4 pre-existing sandbox SVG-rasterization flakes already documented in earlier versions' notes (unrelated to this change), none new. three requests from Andrew, sent together with two
screenshots (verbatim): (1) "site measure app needs the correct user
selector in the startup menu, same way that [UTZLINE Delivery ITP] has,
also needs to be removed from the top menubar in the floor plan as well as
the rename button (see photos)"; (2) "Utzline viewer no longer needs the my
projects option as everything runs through the projects folder ecosystem";
(3) "implement the username as per the delivery itp throughout the entire
system, but instead of it opening a popup, the button is the selector, when
you pick a name it opens a numberpad to input the pin (4 digit pin)."

- **Shared name+PIN identity, ported verbatim from UTZLINE Delivery ITP.**
  The old toolbar "Set your name" button (`userIdentityBtn`, a freeform
  text prompt, no PIN) is gone. In its place, a native `<select>`
  (`#identitySelector`) IS the button — its own dropdown lists every name
  already known in a shared `utzline-users.csv` registry (kept at the
  Projects-root level, a sibling of every project folder, columns
  `Name,PIN,ShowInApps`, PIN in plain text by design — a reference-only
  attribution registry Andrew can hand-edit in a spreadsheet, not real
  access control) plus a final "+ Add a new name…" option. Picking an
  existing name opens a real on-screen numberpad (4-dot progress, digit
  grid, backspace) to enter that person's 4-digit PIN — a wrong PIN shakes
  the dialog and clears for another attempt; a correct one signs in.
  Picking "+ Add a new name…" still asks for the name as plain text (this
  app's own existing "type one thing" dialog, reused via its new
  `okLabel` parameter — see `showRenameDialog()`), then a numberpad to
  choose a PIN, a second to confirm it, then a small "show me in"
  app-tickbox modal (which app(s) this name is meant to appear in —
  purely a reference field for Andrew, never gates anything). This does
  **not** replace the existing per-device `utzline-identity` IndexedDB
  pointer every sibling ITP app already reads (unchanged) — only what
  triggers writing it.
- **Moved to the startup screen, and no longer editor-only.** The new
  identity row (`#identityGateRow`) now lives on the project-gate box —
  above every gate state (Setup/Reconnect/List/Levels/Rooms), in the same
  visual slot the old "My projects" section used to occupy — instead of
  the floor-plan toolbar. **Judgment call:** it is shown, and fully
  functional, in **both** Site Measure and the Viewer. The old toolbar
  button was Viewer-hidden (`VIEW_ONLY_MODE`) since only the editor
  "saved" anything; Andrew's own instruction to roll this out "throughout
  the entire system" (paired with every sibling ITP app already showing
  identity regardless of whether it saves) reads as no longer wanting an
  editor-only carve-out, so `loadDeviceUserName()` now runs unconditionally
  at boot instead of only `if (!VIEW_ONLY_MODE)`.
- **Removed from the floor-plan toolbar entirely:** `#userIdentityBtn`
  (superseded by the above) and `#fileNameBtn` (the pencil "plan"/rename
  button). **Judgment call on the rename button:** only the toolbar
  button and its own click listener were removed — the underlying
  `state.fileBase` rename mechanism (`showRenameDialog()`, used by Save/
  Save PDF/Share/auto-backup for naming) is completely unchanged. The only
  *other* call site for `showRenameDialog()` was the now-removed "My
  projects" → "+ Add a project…" flow's own labeling prompt, so after both
  removals `showRenameDialog()`/`#renameBackdrop` had no remaining caller
  — until the new identity flow above was written to deliberately *reuse*
  it as its own "type a name" prompt (see `beginAddNewIdentityFlow()` and
  `ensureDeviceUserNameForSave()`), so it ended up very much alive, not
  dead code after all.
- **`ensureDeviceUserNameForSave()` (Site Measure's "every explicit Save
  needs a name" rule) adapted to the new registry.** The old freeform
  popup this used to force a name is gone, so this now reuses
  `showRenameDialog()` for the name, checks it against
  `utzline-users.csv`, and routes to the numberpad to verify an existing
  person's PIN or create a brand-new one — inline, without navigating away
  from whatever's open. **Judgment call:** this is not something Andrew
  asked for directly (Delivery ITP has no equivalent forced-name-at-save
  rule), but leaving the old freeform prompt in place here would have left
  two divergent, inconsistent identity paths in one app.
- **Viewer: "My projects" removed entirely** (see the Viewer's own README
  changelog for Andrew's verbatim request — this app shares one source
  file with the Viewer, so the removal lives here too even though it was
  never reachable from the editor build).
- New regression test `run_identity_pin.js` (in `pdftest-projects/`)
  covers the full new flow end to end — add-a-new-name via two numberpad
  rounds + the app-checks modal writing a correct CSV row, picking an
  existing name and verifying its PIN, a wrong PIN being rejected and
  retryable, cancelling reverting the selector, picking a name before any
  Projects folder is chosen being guarded rather than throwing, confirms
  `#userIdentityBtn`/`#fileNameBtn` no longer exist anywhere in the DOM,
  and — against the real built `redline-viewer-pwa/index.html` bundle —
  confirms `#gateMyProjects` and every "My projects" control are gone
  while the new identity row works there too. `run_site_measure_overlay_layers.js`
  and `run_filebase_naming.js` were updated for the new UI (the former's
  "Save with no name set" case now drives the full name+PIN flow instead
  of the old one-step dialog; the latter no longer reads the removed
  `#fileNameBtnLabel`). `run_my_projects_list.js`/
  `run_my_projects_root_folder.js` (testing the now-removed feature) were
  deleted. Full regression suite re-run clean afterward: 82/86 passing,
  the same 4 pre-existing environment-flake failures already documented
  in earlier versions' notes (an intermittent "svg rasterize failed" in
  this sandbox's headless Chromium, affecting PNG-snapshot rendering for
  backups/exports — reproduced identically via a direct call to the
  underlying rasterizer, confirmed unrelated to any identity/CSV code
  touched here), none new.

**v40–v45.6 (2026-09-22 to 2026-09-23):** a large run of releases not
individually logged in this README when they shipped — full detail for
every one of them lives in `next-version-notes.md` in the project.
Headline changes across that stretch: UTZLINE Projects became the
family's real project/level/room/joinery-item creation tool and Site
Measure's own creation UI was cut over to it (v41); a permanent,
multi-user, multi-layer Site Measure overlay architecture shipped, with
per-person draft-vs-saved-layer separation and a toggleable layer picker
(v45.0); flat-project (Projects-created) interop (v43); a shared
`joinery-status.json` pipeline (📏/📦/🚚/🏆) with on-plan status badges,
job notes, and shop drawings (v44–v45.3); "No" answers blocking ITP
sign-off, photo attachments, and REV-numbered shop drawing revisions
(v45.4); UTZLINE Projects' Joinery Register wired to real status/history
(v45.5); and, in this session specifically, the level-list exclusion
list gaining the new `itp-delivery` folder alongside the family's other
ITP-app exclusions (v45.6) — the only change in v45.6 itself.

This folder is the self-contained, installable version of the app. It
was originally built as a separate "UTZLINE Projects" fork of the
older, single-plan **UTZLINE Site Measure** — as of v11, this app IS
UTZLINE Site Measure going forward, and the old single-plan version is
retired. Everything in the app itself (page title, toolbar brand,
opening screen, installed-app name) says "UTZLINE Site Measure" now.

**Each app in the UTZLINE family lives in its own separate GitHub
repository** — this app, the Viewer, and every ITP/Projects/Scheduler
sibling app each have their own repo and their own GitHub Pages URL;
none of them are subfolders of a shared repo. (An earlier version of
this README described a single shared `UTZLINE-Site-Measure` repo with
per-app subfolders — that's no longer how these are hosted; the
sections below describe the current, per-app-repo setup.)

It shares the same underlying markup/photo-annotation and PDF-export
code as the old version, but adds an opening project picker. A project is a folder of
**levels** (e.g. "Level 1", "Level 2") — each level is a fully
independent plan/photo with its own markup, its own `saves/` subfolder
(the working file you reopen) and its own `pdfs/` subfolder (every PDF
you export), all kept apart by project and level name. Everything the
app needs (PDF libraries, fonts) is bundled locally; nothing loads from
the internet once it's cached.

## How the pieces fit together

There are several things in play here, same as with UTZLINE Site
Measure — easy to mix them up:

1. **The claude.ai artifact** — good for a quick look, and it falls back
   gracefully to a normal single-plan view (no project picker) on any
   browser that doesn't support the underlying folder-picker API.
   claude.ai embeds it in a cross-origin iframe, though, and Chrome
   flatly refuses to open a folder picker from a cross-origin iframe —
   so the actual per-project-folders feature only ever works once this
   app is hosted on its own domain or installed. Not what your installed
   copies run.
2. **This bundle, hosted on GitHub Pages** — this app's own repo, live
   at that repo's own GitHub Pages URL. The standalone read-only
   **Viewer** app lives in its own separate repo, not a subfolder of
   this one — see `../redline-viewer-pwa/README.md` for that one
   specifically. This is the real thing: fully offline-capable, and the
   only place the folder picker actually works from a browser tab.
3. **A desktop install** — Chrome/Edge's "Install this site as an app"
   pointed at that hosted URL. Just a shortcut to the same site.
4. **The Android app (the APK)** — also a thin wrapper (a Trusted Web
   Activity) around that same hosted URL, built via
   [PWABuilder](https://www.pwabuilder.com/), with its own package ID and
   its own signing key. **The APK does not contain the app's code.** It
   loads whatever is live at this app's own repo's Pages URL right now,
   so updating the app is a matter of updating the *files* in the repo,
   never rebuilding the APK — except when the app's identity changes
   (name, icon, package ID).
5. **Optionally, chrome-less full-screen mode** (no browser address bar)
   for the Android app — this needs a `.well-known/assetlinks.json` on
   the domain the app claims to represent, verifying the APK's signing
   fingerprint — not set up yet, ask if you want to do this once the APK
   exists.

## Updating the app (this is the main thing you'll do)

Whenever new files show up in chat as a zip:

1. Unzip it.
2. Go to this app's own repo on GitHub — files go at the repo **root**,
   not inside a subfolder (the Viewer, and every other sibling app, each
   have their own separate repo, not a subfolder of this one).
3. Upload the files from the zip, overwriting the existing ones (drag
   them onto the repo page, or use **Add file → Upload files**), keeping
   the `icons` folder structure intact. Commit.
4. Wait about a minute for GitHub Pages to redeploy, then check it took:
   open this app's own Pages URL directly in a normal browser tab and
   confirm the change is there. **Bump the "Current version" line at the
   top of this README (with a dated changelog entry) and
   `service-worker.js`'s `CACHE_NAME` every single time a change ships**
   — both need to move together, or this README stops being a reliable
   record of what's actually live.
5. Get each installed copy to pick it up:
   - **Desktop install**: close and reopen it; a refresh is usually
     enough.
   - **Android app**: fully close it — swipe it away from recent apps,
     don't just background it — then reopen. If it still looks old, do
     that twice, or clear the app's cache (Settings → Apps → UTZLINE
     Projects → Storage & cache → **Clear cache**, not "Clear data" —
     that also wipes any project-root folder permission you'd granted)
     and reopen again.

No APK rebuild, no re-signing, nothing through PWABuilder — that's only
ever needed if the app's *identity* changes (name, icon, package ID),
not for ordinary fixes or features.

## Things worth knowing

- **The project picker only appears where the folder-picker API is
  actually available** — desktop Chrome/Edge, and Android Chrome (from
  a real installed/hosted context, not the claude.ai artifact). On any
  other browser, the app behaves exactly like UTZLINE Site Measure
  always has: it loads straight into a single ongoing plan, no picker,
  no per-project folders.
- **Opening a project takes you to its level list, not straight to a
  plan.** A brand-new project starts with no levels — hit **+ New
  Level** to create the first one ("Level 1", "Ground Floor", whatever
  fits the job). Each level is entirely independent: its own plan
  image, its own markup, its own save file and pdfs history.
- **"Switch level" in the toolbar** takes you back to the current
  project's level list — it'll ask you to confirm first if the current
  plan has unsaved marks on it. From there, **← All projects** steps out
  one more level to the full project picker.
- **Save always drops a fresh, timestamped PDF into the level's pdfs
  folder alongside the plan file** — not just when you explicitly
  export. If either half fails, the toast says exactly which one didn't
  land (rather than a single generic "Saved" that could paper over a
  failed PDF export), so try Save again if you see that.
- **Choosing a Projects folder is a one-time setup per device/browser
  profile.** If permission to it ever lapses (browser data cleared, a
  fresh profile), the app shows a "Reconnect" screen naming the folder
  it remembers rather than silently losing your projects.
- **Creating a project or level with a name that already exists**
  doesn't overwrite it — it silently appends a number (`Building H` →
  `Building H 2`, or `Level 1` → `Level 1 2` within the same project) so
  you never lose an existing one by mistake.

## What's in this folder

- `index.html` — the app itself
- `manifest.json`, `service-worker.js` — what makes it installable/offline
  (the version comment at the top of `service-worker.js` is a running
  changelog of every fix that's shipped)
- `icons/` — app icons
- `jspdf.umd.min.js`, `svg2pdf.umd.min.js`, `pdf.min.js`, `pdf.worker.min.js`,
  `sans.woff2`, `mono.woff2` — bundled libraries and fonts (all local, no CDN)
- `build.py` — regenerates `index.html` from the canonical claude.ai source;
  only relevant if you're working on the code directly rather than through
  chat

## Setting this up fresh (e.g. on a new account/device)

You already have this running, so you shouldn't need this — but for
reference, in case it's ever needed again from scratch:

1. Create a **public** GitHub repo for this app specifically (its own
   repo, not shared with any sibling app). Upload every file from this
   bundle, keeping the `icons` folder structure, at the repo **root**.
   The Viewer app (see `../redline-viewer-pwa/`) gets its own separate
   repo, not a subfolder of this one — likewise for every other sibling
   app in the family.
2. Repo **Settings → Pages** → Source: **Deploy from a branch**, branch
   **main**, folder **/(root)** → Save. Wait ~1 minute for the live URL.
3. Open that URL once while online (to cache it for offline use), then
   install it: on Windows/Mac, the browser's install icon in the address
   bar; on Android, Chrome's **⋮ → Add to Home screen** (or build a
   proper APK via [PWABuilder.com](https://www.pwabuilder.com/) for a
   real installable app with no browser chrome at all — its own package
   ID and signing key, kept separate from other apps).
