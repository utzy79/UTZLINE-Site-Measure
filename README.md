# UTZLINE Site Measure — installable app

**Current version: v45.8** (bump this line, and add a dated changelog
entry below, every time a new build ships — see `next-version-notes.md`
in the project for the full per-version changelog; v40 through v45.6
shipped without this README's own version line being kept in sync, so
that file is the authoritative record for that stretch.)

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
