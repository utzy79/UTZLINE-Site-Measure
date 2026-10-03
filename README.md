# UTZLINE Site Measure — installable app

**Current version: v104 (RC 1.0)** (bump this line, and add a dated changelog

**v104 (2026-10-02): PINs scrambled, blank PIN = choose a new one.** PINs in `utzline-users.csv` are saved scrambled (`h1:<salt>:<sha-256>`), so the file no longer shows them. A plain PIN already in the file still works and is scrambled the next time an app saves the file. A BLANK PIN cell means reset: the next sign-in as that name asks for a new PIN (twice). Update every device before anyone adds a name: an older app cannot read a scrambled PIN. A 4-digit PIN can still be guessed from the file, so keep the file private in OneDrive too.


**v103 (2026-10-02): shorter rework folder names, archiving on.** Rework files now live in `Project Saves\RW\<item>.json`, their history in `Project Saves\RW Log\<item>\` and their PDFs in `PDFs\RW\`, shared by every app (was `Project Saves\UTZLINE ITP\Install ITP Rework`; the old folders are not read). Project archiving is ON: only the Projects app archives or restores (administrator PIN) and asks about projects unopened for 90 days; every other app drops archived projects from its lists (Dollar Summary still counts them).
entry below, every time a new build ships — see `next-version-notes.md`
in the project for the full per-version changelog; v40 through v45.6
shipped without this README's own version line being kept in sync, so
that file is the authoritative record for that stretch.)

**v101 (2026-10-02) — RC 1.0: Viewer plan buttons hidden when no plan is open, Home fixed.**

- **Viewer REWORKS panel: smaller, red / green.** Andrew: *"make these smaller (set by largest writing)"* and *"make them red if there is a rework logged, green if not"*. The buttons are only as wide as the longest label, and each is red when that person / group has outstanding reworks, green when not.
- **Rework PDF header fixed.** Andrew: *"builder logo to be half the size, company logo has disappeared, qr code to be smaller and go down the bottom of the page"*. The builder logo is half the size (at most 75 x 30 pt), the company logo is back (the shared rework module only looked for `company-logo.png` at the Projects root; it now looks in `logos/` first, like every other app), the QR is smaller (48 pt) and sits at the bottom right of page 1 above the footer line (page 1's text and first photo row stop above it), and the status tag no longer covers the "JOINERY REWORK" title. The three ITPs now pass the project's builder logo into the rework PDF too.
- **"Company logo -- set in the UTZLINE Projects app" row hidden.** Andrew: *"hide this on all apps now"*. (This app has a toolbar, not a title header, so there is no header logo here.)
- **Viewer: the plan buttons only show while a plan is open.** Andrew: *"all these buttons should be hidden when the rework register is open in viewer"* and *"anytime a plan is not open"*. On the project list, levels, rooms and any rework register or rework page the Viewer now shows only its name and **Home** (Share, Recolour, select / zoom / pan, layers, help and dark mode come back once a plan is open).
- **Home fixed:** *"keep the home button but fix it"* -- with a rework register or page open, Home used to act underneath it (the register stayed on top, so it looked dead). It now closes the register / page first and lands on the project list with the Reworks panel.

**v100 (2026-10-02) — RC 1.0: Viewer Reworks panel, Change folder pill, Sent to CNC, administrator PIN.**

- **Site Measure shares the rework page: Machine shop and Factory managers buttons** beside the drafters in the Reworks panel (each with its outstanding count), and an *Or flag to* row on the rework page. A rework flagged to a group is taken off its drafter.
- **Viewer (and Site Measure) home screen: Reworks panel.** Andrew: *"viewer should look like this"* (mock-up) + *"those buttons open teh rework, within that they can chose to change the drafter, mark as sent to cnc, add comments"*. In the **Viewer** the home screen is now two columns (the project list as before on the left): on the right a **REWORKS** button and one button per drafter, each with how many reworks are waiting on that drafter (a *Not flagged to a drafter* button when some aren't flagged yet). A button opens the reworks (the register, filtered to that drafter); open one and the drafter can **pass it to another drafter**, mark it **Sent to CNC** (typing the **file name** sent), mark it **Not required**, add comments, and print / share its PDF. Reworks are read in the background for the listed projects only (never their photos). Site Measure doesn't show the panel. The Viewer asks for write permission only when a drafter actually saves something. **Change folder** is now the small bottom-right pill (the in-page "Use a Different Projects Folder" / "Choose a Different Folder" buttons are gone).
- **Change folder needs an administrator PIN, in every app.** Andrew: *"to chose another folder you must enter an administrator pin (on any app)"* and *"Andrew Utz will be the Administrator for now, but possibility to change it later"*. Pressing Change folder now opens a small numberpad that only an administrator's PIN (checked against `utzline-users.csv`) will pass, then the folder picker. The administrators are the names in `utzline-admins.json` at the Projects root (`{"admins":["Andrew Utz"]}`); until that file exists it is just Andrew Utz, and it can be changed later by editing that one file. If the user list can't be read (no folder, permission lapsed, file gone) or no administrator has a PIN in it, the change is allowed so nobody is ever locked out of a lost folder. Reconnect Folder (same folder) is unchanged.
- **"Sent to CNC"** (was "Resent to CNC"): the drafter's answer reads *Sent to CNC* on screen, in the log and in PDFs (the stored value is unchanged, so old reworks still read correctly). Andrew: *"drafter needs an option to mark the rework as Sent to CNC"* and *"and add a filename"* -- the drafter can type the **file name** sent to the CNC beside it; it is kept with the answer and shown in the log, the drafter line and the rework PDF.
- **Rework module:** new event kind `cutNotRequired` (Machine Schedule) and the file name on `drafterAction`; every app reads and shows them even where it can't write them.

**v99 (2026-10-02) — RC 1.0: each rework has its own QR code, rework PDF header redesigned.**

- Andrew: *"can each rework have its own qr code"*. Every rework's own PDF now carries **its own QR code** (top right of page 1, "Scan to open this rework"). Which ITP it opens follows the rework's **stage** (Andrew: *"it will depend on the status"*, then *"also need to think about the manufacture itp"* -> *follow the stage*): logged or cut -> **Manufacture ITP**, complete / ready to deliver -> **Delivery ITP**, delivered or closed out -> **Install ITP**. The link names the rework (`&a=rework&w=<id>`; spaces as `+` so the code stays small). A hosted copy opened from a phone-camera link hands off to the right ITP by the rework's stage (`&h=1` stops it going back and forth); the app's own Scan button just opens it where it is. The item's Rework screen opens with **that rework's card scrolled into view and ringed**. Shared `UtzQr.reworkLink / reworkApp` and `UtzRework` (every app that makes a rework PDF now carries the QR module).
- **Rework PDF header like Andrew's picture**: company logo top left, the builder's logo and — always — the **UTZLINE logo** top right (the UTZLINE mark in every app, no longer the app's own icon), the rework's QR in the gap between them, the title row and status tag underneath. With no company logo the QR makes the header a little taller, so the first row of photos may start on page 2.

**v98 (2026-10-02) — RC 1.0: the kind of drawing is in the file name.**

- Andrew: *"change JN to IFC"*. Job notes are now **IFC** (Issued For Construction): the folder under `PDFs\` is `PDFs\IFC\<Level>\<Room>\<item>\` (was `PDFs\JN\...`), and a job note's file name carries ` -- IFC -- ` (`<project> -- IFC -- <room> - <code> - <saved>.pdf`; Site Measure and the Scheduler write them). Shared folder code (`UtzItemFiles`), so every app reads the same place; the code still says "JN" internally, only the folder and the tag read IFC. Old `PDFs\JN` folders are not read any more (Andrew: happy to lose old files as long as new ones work); his existing 3749 job notes were renamed and moved to `PDFs\IFC`.
- Andrew: "we need a way to decifer if a drawing is a check measure, job note etc ... like this 3749 - New Mount Barker Hospital -- CM -- H1.AH.010 - J.170 - 2026-10-02 15-40-29". Check measure PDFs are now `<project> -- CM -- <room> - <item> - <date>.pdf` (a room's own site measure `<project> -- CM -- <room> - <date>`, a level's plan `<project> -- CM -- <level> - <date>`) and job notes `<project> -- JN -- <room> - <item> - <date>.pdf`. The " -- CM -- " / " -- JN -- " (double dash) marks the kind and is easy to search for. Short on path (Windows' 260 limit) the project part gives way first (to 8 characters), then the room part, then the project altogether; the item code is never cut before them. Files already saved keep their names. Shop drawings keep "REV n - <date> - <code>.pdf" -- every app reads the revision order off the start of that name, so it is not changed here.
- Tests: `run_sm_code_only_names.js`, `run_v65_jobnote_for_construction.js`, `run_joinery_status_and_job_notes.js`, `run_viewer_item_picker_v94.js`.

**v97 (2026-10-02) — RC 1.0: job notes and shop drawings are in room folders (with every other app).**

- Andrew: "i want every room to have a folder, then the joinery within it", then "im happy to lose old files, as long as new ones work". Job notes are saved in `PDFs\JN\<Level>\<Room>\<item>\` and shop drawings in `PDFs\SD\<Level>\<Room>\<item>\<drawing>\[Returned|Approved]\`, next to the check measures in `PDFs\CM\<Level>\<Room>\<item>\`; the room folder is the room number, the level is cut to 30 (the same names as the event folders). The old `Project Saves\Job Notes` and `Project Saves\Shop Drawings` folders are no longer read (nothing moved or deleted). "Which items have a shop drawing" scans the new layout. The 260-character path check counts the new folders. Scheduler, Machine Schedule, Solid Surface, Projects, Dollar Summary and the three ITPs changed in the same round.
- Tests: `run_sm_code_only_names.js`, `run_v65_jobnote_for_construction.js`, `run_joinery_status_and_job_notes.js`, `run_shop_drawings.js`, `run_v64_shop_drawings_sent_returned.js` (A/B), `run_viewer_shop_drawing_upload_v81.js`, `run_viewer_item_picker_v94.js`.

**v96 (2026-10-02) — RC 1.0: every room has its own folder for check measure PDFs and backups.**

- Andrew: "i want every room to have a folder, then the joinery within it", "or even PDFs\", "and do the same with backups". Check measure PDFs now go to `PDFs\CM\<Level>\<Room>\<Joinery item>\` and backups to `Backups\CM\<Level>\<Room>\<Joinery item>\` (each folder still keeps its 10 newest backups). A room's own site measure is saved in `PDFs\CM\<Level>\<Room>\` (and `Backups\CM\<Level>\<Room>\`); a level's plan in `PDFs\CM\<Level>\`. The room folder is the room NUMBER, as in the file names; an item with no room goes in "No room". File names are unchanged (still `<project> - <room> - <item> - <date>`), so a PDF that is emailed on its own still says what it is.
- The old `PDF Files\UTZLINE Site Measure\` and `Backups\UTZLINE Site Measure\` folders are no longer written to and nothing in them is moved or deleted (the app never read them back). Job notes and shop drawings keep their present folders for now — moving them changes every app, so it is a separate step.
- The 260-character path check counts the new folders (for level plans as well as items).
- Tests: `run_sm_code_only_names.js`, `run_flat_structure_interop.js`, `run_sm_room_measure_v89.js` (new check N), `run_v46_site_measure_round.js`, `run_v54_check_measure.js`, `run_viewer_item_picker_v94.js`.

**v95 (2026-10-02) — RC 1.0: the room number goes between the project name and the joinery code in PDF names.**

- Andrew: "the pdf files need the room numbers / names between the project name and the joinery code, does this risk the 255 character rule". Chosen: Site Measure exports and job notes; the room NUMBER only; a room's own page has no single code. An item's check-measure PDF is now "<project ≤40> - <room number> - <item file name> - <date>.pdf" (was "<project> - <item file name> - <date>.pdf"); a room's page is "<project ≤40> - <room number> - <date>.pdf" (was "... - Room - <level> - <room> - ..."); a job note is "Job note - <date> - <room number> - <code>.pdf" (the .json beside it the same). The room number is the first part of the room's name that has a digit (H1.MH.040, 101); a room with no digit in its name uses the name, cut to 14 characters. Level plan exports, shop drawings and the folder names are unchanged. 255 / 260 rule: the room number adds at most 17 characters (14 + " - "). The project's measured path budget (the same probe the job notes use) is applied: the PDF's project part is cut first (down to 8 characters), and a job note drops the room part before it would ever cut the code; the code always stays whole. Exports on a project folder with a long path were already close; the new worst case is about 114 characters of project-folder path for the PDF folder at the longest names (it was 131). Tests: run_sm_code_only_names.js, run_job_note_no_plan_leak.js, run_v65_jobnote_for_construction.js, run_joinery_status_and_job_notes.js updated.

**v94 (2026-10-02) — RC 1.0: Add shop drawing / Add job note ask which items in the room the file is for.**

- Andrew: "30% opaque indicator — add shop drawing or job note should reflect the same (selector to choose what items in the room the drawing are for)". In the Viewer's **Add shop drawing** and **Add job note** dialogs, when the marker's room has more than one item, the same plan preview + tick-list as the room's Save & exit sits in the dialog ("Which items in ROOM X is this for?"): the item you opened starts ticked (solid, orange ring), the others start unticked (faint, 30%). Shop drawings: the file is saved on every ticked item (the opened item keeps its REV / returned-of logic; the others get it as a new drawing, as a drawing-group member always did; a group member in ANOTHER room still gets it as before). Job notes: one note per ticked item, each with its own plan pages; "Job note added" is shown for the opened item, listing the rest. Nothing ticked: Save is disabled / the drop is refused ("Tick at least one item"). A room with one item shows no selector. The preview exists only while the dialog is open. Site Measure carries the code but neither dialog (Viewer only). Test `run_viewer_item_picker_v94.js`.

**v93 (2026-10-02) — RC 1.0: the room tick-list shows a plan preview so you can check you've ticked the right item.**

- Andrew: "when ticked, it shows the indicator overlay so you can ensure you have ticked the right one" — chosen: a plan preview in the dialog; "only while the tick menu is open". The room's Save & exit tick-list now has a small picture of the level plan around the room (the plan remembered when the room page was opened), with the room's markers drawn on it: unticked ones faint (30%), ticked ones solid with an orange ring and their code. It updates as you tick / Tick all / Untick all and disappears the moment the list closes (nothing is added to the page or saved). Items with no marker on the plan are listed but can't appear in the picture. Test `run_sm_room_measure_v89.js` (K–M).

**v92 (2026-10-02) — RC 1.0: the room tick-list starts with nothing ticked.**

- Andrew: "start unticked". On a room's Save & exit, every item in the tick-list now starts UNticked (items already check measured stay greyed); tick the ones that are done, then **Mark ticked**. Tick all / Untick all still there. Test `run_sm_room_measure_v89.js` (G–J updated).

**v91 (2026-10-02) — RC 1.0: on a room's site measure, Save & exit asks which items are now check measured.**

- Andrew (after trying room site measures): "can we still have an option to mark individual items in the room as measured". The per-item **Mark as check measured** (and **Site measure not required**) rows in the marker menu are unchanged. New: on a ROOM page, Save & exit no longer asks the single Yes/No "Mark check measure complete?" (which had no real item to mark there); it shows a tick-list of the room's items — markers on the plan ticked, any other item the project lists for the room unticked, items already check measured greyed — with **Mark ticked** / **Mark none**, plus Tick all / Untick all. Only the ticked items are marked; the toast says how many. An item's own page (older per-item measures) keeps the Yes/No. Tests `run_sm_room_measure_v89.js` (G–J).

**v90 (2026-10-02) — RC 1.0: the right-click menu says which room is being site measured.**

- Andrew: "we need a way to know that we are doing it as a room when there are merged items. this popup should maybe say in bold that we are site measuring the room (with room number)." The marker menu now shows a bold line under the item title, **Site measure: ROOM <room>**, and — when more than one marker on the plan belongs to that room — "One site measure covers all N items in this room". Not shown for an item with no room, or one still on its own older site measure. Test `run_sm_room_measure_v89.js` (E, F).

**v89 (2026-10-02) — RC 1.0: one site measure per room.**

- **One site measure covers every item linked to a room** (Andrew: "if a room has linked items, one site measure covers them all"). Create / Open site measure on ANY item of a room opens the same page, kept in `Project Saves/Site Measures/Room - <level> - <room>/` and named "<Level> — <Room>"; the measures drawn on it by anyone show as layers as before. An item with no room keeps its own page. An item that already has its own older site measure, in a room that has none yet, keeps opening that one, so earlier work is never hidden. Job notes, shop drawings, status and every event log stay per item. The Viewer's joinery summary lists the room's measures and the item's older ones. Test `run_sm_room_measure_v89.js`.

**v88 (2026-10-02) — RC 1.0: crop window is a real modal, no drawing on the main plan, callout number pad.**

- **Crop window (Create site measure)**: it now covers the whole screen, toolbar included (before, the toolbar and plan behind it still took touches, so an accidental drag panned the plan and could blank the page). Nothing pans / zooms / draws behind it; the plan is put back exactly as it was; the device Back button or an edge swipe **cancels** the open (stay on the plan, nothing changed) instead of carrying on half-way.
- **No site measures on the main plan**: on a level (main) plan the Dimension, Line, Angle, Rectangle, Text and Callout tools and Insert image are hidden (and ignored: shortcuts, paste, file insert). Select and Pan stay. Every tool is available on a joinery item's own site measure page.
- **Callout** labels use the same on-screen number pad as a dimension (with ABC for the full keyboard).
- Tests: `run_sm_crop_drag_touch_v88.js`, `run_sm_crop_back_v88.js`, `run_sm_level_lock_v88.js`, `run_v59_measure_pad.js` (callout now gets the pad).

**v87 (2026-10-02) — RC 1.0: sharp crop on a new site measure.**

- **Create site measure → crop**: the crop window is now shown on the quick preview while the level plan is still on screen, and the area you pick (or "Use full view") is then rendered again from the plan at its own full resolution (up to 4096 px on the long edge) instead of being cut out of the screen-resolution preview, so the new page is no longer blurry. Test `run_sm_crop_sharp_v87.js`.

**v85 (2026-10-02) — RC 1.0: logos folder, reversed Machined, drag and drop only, Viewer button ink.**

- **Company and builder logos live in a `logos` folder** at the Projects root (reads `logos/` first, falls back to the old root files; `logos` is never listed as a project; the "Get ready for offline" check and the sync note include it).
- **Reversed Machined**: the status honours the Machine Schedule's `statusRetract` event (the item page and summary show the earlier stage again; history shows the entry struck through, then "reversed").
- **Drag and drop only -- no file selector**: job notes, shop drawings (the Add dialog and the summary card) are drop boxes; Insert image takes a photo / PDF dropped on the plan or pasted with Ctrl+V (the picker only opens on a touch-only tablet, where nothing can be dropped, via a long press).
- **Viewer**: text on the bright dark-mode blue buttons (Save, Choose Projects Folder, Reconnect, Confirm, the layers badge) is now dark -- white on that blue was 2.3:1, "white writing on a pale background".

**v84 (2026-10-02) — RC 1.0: Share from the check measure, builder logo at the far right of the top bar, older measure layers off by default (and deletable in Site Measure), the Viewer draws the latest measure exactly like Site Measure, Viewer room list.**

- **Share** is always on the toolbar now (it used to appear only where the device has a share sheet). On a check measure it offers "Current view" (a picture) or "Full check measure (PDF)". Where there is no share sheet (the PC app) it saves the file to Downloads instead.
- **Builder logo** sits at the far right of the top bar, on the same row as the title, and stays at the right edge when the bar is scrolled sideways.
- **Layers:** the newest save is drawn on the page exactly as Site Measure draws its own marks (bold text, halo, original colours) whenever the page has no marks of its own (always in the Viewer); every older save is a hidden reference layer -- tick it in the layers panel to audit it (tinted, thin). When you have your own save on the page, every other layer starts hidden.
- **Delete an old layer (Site Measure only):** a Delete button beside each layer that a newer save has overwritten; asks first, one at a time, never offered for the newest, never in the Viewer. The file is moved into a "Deleted layers" folder inside the item's own folder, with a small record of who deleted it and when -- nothing is erased.
- **Viewer: Room list** button (on a level's plan): the rooms on the level, alphabetical, with item counts; tap one to go to it on the plan.
- Sign-in "Opening…" cover and the shared rework module's "Logged in" display re-inlined.

**v83 (2026-10-01) — RC 1.0: the plan pages travel with the job note; Export floor plan works from the summary; status line for shop drawings (Viewer).**

- **Job note + plan pages (Viewer).** Adding a job note now draws the two A3 landscape plan pages (the close-up around the item, then the whole level plan, each with the indicator and both QR codes) and puts them at the **end of the same PDF**, after the For Construction watermark (so they are never watermarked). One file, no separate plan export per item in the project folder (Andrew: *"dont duplicate that export per joinery item in the files"*). The note's `.json` carries `withPlan: true`. If the plan can't be drawn or joined, the note is saved as before and the popup says so.
- **"Print entire job note"** on the popup shown after a note is added (and on a manual floor plan export): prints the job note and its plan pages together. From a manual export it finds the item's newest job note and joins it to the pages (a note that already carries its plan pages is printed as it is; no note: the plan pages only). On a computer it opens the print dialog; on a tablet it opens the PDF for the viewer's own Print.
- **Export floor plan (A3 PDF)** no longer writes a file into `PDF Files/Floor Plans`; the popup offers Open / Share / Print.
- **Bug fixed: "Export floor plan" did nothing until you went back to the plan.** The popup (and the "Exporting…" toast) were being drawn *under* the joinery summary screen, so it looked dead until the summary was closed. They now sit above every full screen.
- **Plan exports show the indicator and joinery code, not the status icon** (Andrew: *"show the indicator and joinery code, but not the status icon"*).
- **Viewer joinery summary: a shop drawing status line** — a coloured pill (Sent / Returned to resubmit / Approved, with its REV, who and when) in the details list and at the top of the Shop drawings card, from the newest of the latest sent revision, returned copy and approved copy (the Scheduler has had this since v46).

**v82 (2026-10-01) — RC 1.0: the red "Add rework here" QR code on the plan PDF export.**

- **The plan PDF export** (Site Measure / Viewer) has a **second QR code**, dark red, at the bottom right of each page under the first one's column and labelled **Add rework here** (Andrew: *"another qr code that takes you to the add rework option ... bottom of the page under right aligned with the current one and a different colour (red if possible) labeled add rework here"*). It carries the same item link with `a=rework`; scanning it (or opening it from the phone's camera) in the Install or Delivery ITP goes straight to that item's Add rework screen; the Manufacture ITP (no rework screen) opens the item and says rework is added in the Install or Delivery ITP.

**v81 (2026-10-01) — RC 1.0: code-only file names -- joinery codes, not descriptions, in every file and folder name (path-limit round, fourth build).**

- Andrew: *"have a real good think about how we can minimise filepaths, maybe we need to lose the joinery descriptions and just have joinery codes. give me a solid solution"* -- then *"I have no actual current files so dont care if I need to start again"*. Every folder and file kept for ONE joinery item is now named by the item's **file name** -- its joinery code (e.g. `JG.33.1`; a second item with the same code is `JG.33.1 (2)`), chosen once by UTZLINE Projects when the item is made or imported and saved on the item in `joinery-items.json` (`fileKey`), never changed afterwards -- instead of `<Level> - <Room> - <Code>` (64 characters for the pilot's `Ground Floor - G.33 - Change Cubical & Patient Consent - JG.33.1`). The level, room and description stay inside the records and `joinery-items.json`, so every screen still shows them.
- Records carry the author's **initials** and a two-digit-year stamp (`JG.33.1 -- AU - 26-10-01 16-25-35-281 - set.json`, in a level folder cut to 30 characters); imported files are **renamed** on the way in (the name they came in with is kept in the record or the `.json` beside the file and is what the screen shows). On the pilot's own folder (88 characters) the longest path is now 121 of the 163 the project folder leaves -- project folders up to about 130 characters deep work.
- **Clean break:** nothing is read under the old long names. Set the project up again in UTZLINE Projects (it gives every item its file name when the project is opened) -- the other apps pick the names up from `joinery-items.json`.
- Site Measure: Site Measure overlays/drafts, PDFs and job notes use the item's file name; overlay files are `<initials> - <yy-mm-dd hh-mm-ss-mmm>.json` with the full name inside (`savedBy`).

**v80 (2026-10-01) — RC 1.0: path-limit round, third build — the folder-path banner only when records really can't fit.**

- Andrew, with the banner on screen (*"leaves only 163 for UTZLINE's own files (long room names need about 170)"*): *"what can we do, im already in the root folder for onedrive"*. 163 is plenty for the short record names -- his longest record is 150 characters after the project folder (141 with the shortest branch) -- the 170 was set for the old long names. The banner now shows only when the project's folder path leaves less than even the shortest names need (**135**), and then says so (*"even the shortest record names need about 135, so some records may not save"*).
- A record that does have to take a short `_hash` name (a very long room-and-code) is saved and read like any other, so it no longer raises the banner: the page gets a `utz-path-limit` event (why `fallback`) and a console note. The banner stays for a record that **can't** be saved at all.

**v79 (2026-10-01) — RC 1.0: path-limit round, second build — `~` is a name Chrome refuses; record names with initials and a two-digit year.**

- Andrew's first run of the morning build showed the banner with *"leaves only 0"*: Chrome's File System Access API refuses any file name containing a `~` (it treats the tilde as a reserved Windows character), so every probe file -- and the `~hash` fallback names -- would have been refused. The markers are `_` now (`_hash`, 9 characters) and the probe name has no tilde; measured on the pilot folder the real figure is 163.
- Andrew: *"change usernames to initials, year from 2026 to 26, remove milliseconds?"* -- a record's on-disk name part is now `<initials> - <yy-mm-dd hh-mm-ss-mmm> - <kind>.json` (`AU - 26-10-01 13-09-18-862 - set.json`; the record's body still carries the full name and time, every reader folds from the body). The milliseconds stay: two saves in the same second must never land on one name. Together with the level no longer in the name, the pilot's longest record is 141 characters after the project folder (was 166; it has 163).

**v78 (2026-10-01) — RC 1.0: Windows' 260-character path limit — records and backup snapshots for long room names were being saved empty.**

- **Why:** Andrew: *"some get corrupted from the import ... then i cant change them in the schedule"* / Set schedule's *"Couldn't save this schedule"*. Windows limits a file's full path to 260 characters; the pilot project's folder path (`C:\Users\andrewu\OneDrive - Metro Joinery\UTZLINE Pilot\3756 - Jones Radiology Mt Barker\`, 88 characters) left a schedule record for a long room name at 253 in full -- Chrome's `<name>.crswap` swap file needs 7 more, so the file was created EMPTY and the save failed. 27 of the 72 schedule files in that project's Ground Floor folder were empty.
- **Event store (shared with every app that writes records):** a new record's name no longer repeats the level its folder names -- `UTZLINE Events/<Branch>/<Level>/<Room> - <Code> -- <name> - <stamp> - <kind>.json`; everything already on disk under the long name still reads. When even that doesn't fit, the empty file is removed and the record is kept under a 9-character `~hash` name, and a banner says why; on a PC the first write to a project measures what its folder path leaves and the banner shows early when it is under the ~170 characters long room names need (*move the Projects folder nearer the drive root, or shorten the project folder's name*). Update every device: an app on the previous version doesn't see records under the new short names.
- **Backups:** a level / room / joinery item's rolling backup snapshot is now `backup_<date>_<time>.utzline.json` (+ `.png`) inside its own folder under `Backups/UTZLINE Site Measure/` -- the name used to repeat the project and the item (219 characters after the project folder for a long room name: those snapshots were being left empty on PCs). Pruning orders old and new names by their date-time.
- **Job notes:** the original file name is kept to 40 characters in the saved name.
- Test: `pdftest-projects/run_event_store_path_limit.js` (a mock folder that behaves like Windows: Andrew's path, a deeper one, a hopeless one, and Linux).

**v77 (2026-10-01) — RC 1.0: the Viewer's floor plan export carries a QR code (Site Measure only carries the code).**

- Andrew: *"can that viewer export also generate and apply a qr code on the page, that the delivery itp can scan to open the relevant room / joinery item"* → *"All three ITPs"*. See the Viewer's README; nothing changes in Site Measure's own screens. Shared code now carried by both builds: the QR encoder (`shared/qr/qr-code.js`).

**v76 (2026-10-01) — RC 1.0: Viewer round — joinery summary, Export floor plan after a job note (Site Measure only carries the code; its own menu is unchanged).**

- Andrew: *"build that in the viewer, (export floor plan) plus the viewer to have the right click option to open the joinery summary"* / *"that should have been when job note is added. create the a3 plans"*. The new rows and the after-job-note popup are on the **Viewer** only — see the Viewer's README. Site Measure's menu is as before.
- Shared code now carried by both builds: the ITP log reader and the item-extras cards (Cutting file / Notes / History).

**v75 (2026-09-30) — RC 1.0: one-finger pan, blue markers 25% smaller, the room in the marker menu, the builder's logo, the drafter step on reworks (Viewer).**

- Andrew: *"all floor plans should pan / zoom with the same functionality (1 finger scroll, pinch to zoom)"*: with the **Select** tool a drag that starts on empty plan now **pans** (a tap still selects the photo / deselects; dragging an object still moves it; the Pan tool, Space and the middle button work as before).
- Andrew: *"all icons to have this fill colour as default"* + *"make the text and icons 25% smaller"*: plan markers are drawn the Site Measure way (white ring, black ring, **blue #0011ff** fill, whatever colour they were saved with), the dot and the label text 25% smaller (the status icon follows the dot).
- The marker's right-click / long-press menu now shows the **room** as well as the joinery ID (Andrew: *"these menus to show the room number also ... across all apps that have these popups on right click"*).
- The day / night button now shares its choice with the other UTZLINE apps on the device.
- **Builder's logo** beside the project name in the top bar (set up once per builder in UTZLINE Projects).
- **Drafter step on reworks** (Viewer; Andrew: *"under view reworks here put the names of the people that have uploaded drawings into this project, then in that flag the rework, within that rework the person can either mark it as resent to CNC, Not required ... or pass it onto another drafter"*): the reworks list names the drafters (the people the Scheduler has recorded uploading shop drawings into the project; the shared name list until then); a rework can be flagged to one; on the rework page the drafter answers **Resent to CNC**, **Not required** or **Pass on to…** -- each its own event file, shown in the status log everywhere and on the rework PDF ("Drafter").

**v74 (2026-09-30) — RC 1.0: sign in every time the app is opened (tablets and phones).**

- Andrew: *"on next update, when opening the apps, it should as[k] for you to login, currently it just loads to the last user that was logged in, some of these tablets will have multiple users (employees)"*. **On a tablet or phone the app now asks who is using it** -- a full-screen *Who's using this?* list (every name in `utzline-users.csv`, plus *+ Add a new name…*) each time the app is opened, and again when it has been in the background for **10 minutes or more**. Tap your name and enter your 4-digit PIN on the usual numberpad. The name saved on the device is only treated as "the last person" now; if another app on the device signs in as someone else, this one asks again when it comes back to the front. **A PC is unchanged** (it keeps the last user), and the PIN numberpad, the registry and the name stamped on saves are as before.

**v73 (2026-09-30) — RC 1.0: records are kept one folder per level — much faster on a tablet; less loaded at start.**

- Andrew: *"how can we speed up schedule loading on the app android"* / *"all are slow"*. Every status, schedule date, cut, solid-surface tick, cutting file and note is still one small file per change (nothing is ever rewritten), but they now go in **one folder per level** — `Project Saves/UTZLINE Events/<record type>/<Level>/`, each file named `<Level> - <Room> - <Code> -- <name> - <time> - <kind>.json` — instead of one folder per joinery item. A schedule now lists a handful of level folders instead of hundreds of item folders; on the tablet each folder costs about a quarter of a second.
- Records a project already has in the old item folders are still read, and both places are shown together (a record found in both counts once). UTZLINE Projects shows **Speed up this project** on a project that still has old folders and moves them — each record copied, checked, then its old copy removed.
- **Update every tablet and PC.** An app older than this one doesn't look in the level folders, so it won't see records written by this one — and only press *Speed up this project* once every device is updated.
- **The PDF tools load when they're first needed** (Andrew: *"Is there anything we can strip out to speed it up. Any bloat"*). jsPDF, svg2pdf and pdf.js used to load every time the app opened, about 1.2 MB of code parsed before anything showed; now it loads the first time a PDF is made or a PDF plan is opened. Offline it still comes from the app's own copy.
- **On a tablet or phone, auto-backup just saves the level.** It used to also draw a PNG of the plan and write it with a full copy of the level into the level's backup folder (10 kept) every few minutes — heavy on a 4 GB tablet, and every snapshot then synced up and down to every device. A PC still makes the snapshots; the tablet shows "Saved." instead of "Backed up.".


**v72 (2026-09-30) — RC 1.0: "Get ready for offline" is quick on Android.**

- Andrew: *"it has taken 10 minutes to "get ready for site""*. The check opened every file in the ticked jobs one at a time, and on his tablet each open takes about a quarter of a second. On an Android tablet OneSync / Dropsync keep a real copy of every file, so there's nothing to download: it now just checks the ticked jobs and the names & PINs list are on the tablet (a few seconds) and says so. **Open every file (slow)** in that dialog still does the full check. Windows laptops (OneDrive "online-only" files) still get the full check, which downloads what's missing.

**v71 (2026-09-29) — RC 1.0: shop drawing revisions start at REV 0.**

- Andrew: *"all revisions start at REV 0  Not REV A  It goes 0 A B C D E etc..."*. A drawing's revisions are put in the order they were saved and labelled by position: REV 0, then REV A, B, C … A first revision saved as "REV A" before today now shows as REV 0; nothing on disk is renamed. A returned copy shows the label of the revision it answers. The same in Projects, the Scheduler and Machine Schedule.

**v70 (2026-09-29) — RC 1.0: the factory for In manufacture, every time; every save retried.**

- **🏭 for a job note too.** Andrew: *"viewer is giving me different icond for in manufacture, some of it if th ehammer and spanner, others is the factory, i want the factory throguhout"*. A job note means In manufacture, but an item whose In manufacture step hadn't landed (the Scheduler's job-note bug, fixed in Scheduler v35) or that predates the rule showed 🛠️ on its marker. It now shows 🏭 like the rest.
- **Every save is retried and checked.** Status events, check measures, overlays, backups, the names list and saved PDFs / PNGs: each is read back to check its size, and the whole write is tried again 0.5 s and 1.5 s later if it fails. On Windows, a sync client or antivirus holding a brand-new file for a moment used to fail the save and leave a 0-byte file with nothing said.
- **No message says "still syncing?" any more.** It was a guess and usually wrong. Messages now say "couldn't read … just now".

**v69 (2026-09-29) — RC 1.0: Projects on this device.** Andrew: *"the onsite apps need an option fo rthe user to pick the projectas they are working on to minimise the syunc on their device"*.

- **Plan markers 20% smaller.** Andrew: *"on he next projects update, make the indicator dots about 20% smaller (and the icons)"*. Every marker dot on a plan, and the status icon over it, is drawn at 0.8 × its saved size. This matches UTZLINE Projects v42. Nothing saved changes, and tapping a marker still uses the full size.
- **Projects on this device.** The project list has a new bar at the top: **Choose my projects**. Tick the one or two jobs this device is working on. Then:
  - Only those are listed. The rest sit behind **Show the other N projects**, and the app reads nothing from them. On a Windows tablet with OneDrive, an online-only job is never downloaded by this app.
  - **How to sync only these** says exactly what to keep on the device: the files in the main folder itself (names & PINs, logo) and each ticked job. It covers OneDrive on Windows (Always keep on this device / Free up space) and OneSync / Dropsync on Android (sync only those folders).
  - **Get ready for offline** opens every file in the ticked jobs, plus the names & PINs list. Anything online-only is downloaded while there's internet. Anything that won't open is listed. The bar then shows "✓ Ready for offline — checked today 09:15".
  - A ticked job that isn't on the device shows as **not on this device yet**, not just missing.
  - The choice is kept on this device, per main folder, under the same key in every UTZLINE app. Nothing is written to the Projects folder. **Show all projects** in the chooser goes back to the full list.

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
