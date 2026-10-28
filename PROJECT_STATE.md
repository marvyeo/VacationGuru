# VacationGuru — Project State

> **Next chat name: `VacationGuru ver 6`**
> Claude: your FIRST reply in a new chat must start with "Rename this chat to: **VacationGuru ver 6**" (you can't rename chats yourself). In your first commit this session, bump the number here by 1.

**State version:** 3 (ver 1 written 2026-10-20 in "Life Plan Roadmap ver 2"; moved into the repo 2026-10-22 by "Life Plan Roadmap ver 3")
**Where this lives:** `PROJECT_STATE.md` in the repo. New chats `git pull` and read it; no uploads needed.
**Project status:** ⏸ **ON HOLD after the Perth trip (2–8 Nov 2026).** No further holidays are planned for a while. Resume when the next trip is being planned.

## ⚙ Session Rules (every chat that works on this app follows these)

**1. Pick the cheapest model that can do the job.** At the start of every session Claude names the model it recommends and the estimated size (below). If the chat is on a pricier model than needed, Claude says so before starting.

| Model | Use for |
|---|---|
| **Haiku** | State-file edits, text or label changes, small data packs, lookups, "where is X" questions |
| **Sonnet** (default) | Features, bug fixes, refactors, data processing (e.g. novel volumes, PullList), most app work |
| **Opus** | Only for new-app builds, cross-app architecture, or a bug Sonnet has failed on twice. Claude says why before using it |

**2. Every push updates this file in the same commit.** No exceptions, including PR branches and one-line fixes.
- `git pull` before starting, so you build on the latest state.
- Add a dated line to the change log (what changed and why, in plain words) and update any section the change made wrong.
- If a session ends without a push, nothing changes here.

**3. Size the work first, and plan the pause points.**
- Before starting, Claude lists the steps and sizes the job: **S** (under ~20k tokens), **M** (~20–60k), **L** (over ~60k).
- **M and L jobs are split into checkpoints.** Each checkpoint ends with a commit and a state update, so a cut-off session loses nothing.
- **Pause** when usage is around half the window, when the chat has grown long (heavy file reads, ~30+ turns), or before a step that won't finish in what's left. Claude can't see the remaining credit directly, so on L jobs it asks Marvin to check usage at each checkpoint.
- **On pause,** Claude writes a `▶ RESUME HERE` block at the top of this file: what's done, the exact next step, branch and files, and the first command to run. It also gives Marvin a one-line restart prompt ("Continue <App> ver X from RESUME HERE"). The next session starts from that block and deletes it once the step is done.

**4. Chat naming.** Use the **Next chat name** line at the top of this file; tell Marvin in the first reply, then bump it.

---

## 0. Instructions for the New Chat (read first)

1. Read this whole file before doing anything.
2. Ask Marvin where the current `index.html` is (see Section 2). Do not rebuild from scratch unless he says the file is lost. Once found, commit it here.
3. Ask which job he wants:
   - **A. Close out Perth:** lock the trip and run the ratings pass.
   - **B. Plan a new trip:** use the skeleton builder from Perth.
   - **C. Deploy and integrate:** GitHub Pages, plus `upcoming[]` for Life Plan.
4. Bring decisions that affect other apps (schema, `upcoming[]`, Drive filenames) back to the **Life Plan Roadmap** chat.

---

## 1. What VacationGuru Is

A single-file trip planner (vanilla JS, Tailwind CDN, Playfair Display and Inter fonts, deep slate navy with warm sand palette, "travel journal" look). It is part of Marvin's Life Plan app ecosystem.

**Features built (original build chat, Sept–Oct 2026):**
- **Trip lifecycle:** Planning → Active → Locked.
- **Day-by-day itinerary:** typed stops (accommodation, dining, activity, transport, booking) with time, notes, cost, link and rating.
- **Bookings tracker:** confirmation number, cost, deadline and booked flag, with deadline urgency badges.
- **Lock mechanic:** locking a trip triggers a post-trip ratings pass. Locked trips are read-only reference data; ratings and actuals notes stay editable.
- **Build from skeleton:** a new trip inherits tags, budget tier and highly rated stops from a locked trip.
- **Google Drive sync:** shared OAuth Client ID `669494100250-gvromuq72r7j0djmkd4vdbdq5damncj7.apps.googleusercontent.com`, scope `drive.file`, file `vacationguru-data.json`.
- **Booking details (added 2026-10-23, data v2):** each booking has a category (flight, stay, car rental, activity, other), booking reference, booked via (Direct, Trip.com, Agoda, Klook or free text), pax covered and cost. Flights add carrier and seats.
- **Start and end times:** every itinerary stop has Starts and Ends times (e.g. flight departs and arrives).
- **Linked bookings:** a stop can link to one or more bookings and shows their cost, source, pax, reference and seats underneath.
- **Checklist tab:** grouped to-do items with optional due dates. Undone items with a due date feed `upcoming[]`. New trips copy the checklist from a skeleton, with everything unticked.
- **Timeline layout (2026-10-23):** start time on the left, then end time and duration (e.g. `5h 15m`). A stop with no end time shows `~end` and `~duration` inferred from the next stop. Day headers show an optional day title.
- **On-the-day spend:** a stop's cost field is now "Spend" (extra money on the day). Stops linked to a costed booking show a "＋ Add on-the-day spend" prompt until a value is entered (0 dismisses it). Summary shows it as "On-the-day spend".
- **More stop types:** shopping and rest, alongside stay, dining, activity, transport, booking.
- **Import (2026-10-23):** Import button on the trips list. Paste JSON or choose a .json file. Format `vacationguru-trip` v1, with trip, days, stops, bookings and checklist. Stops link to bookings by matching words in booking titles. If the trip `id` already exists, choose Update itinerary (keeps bookings, checklist, notes) or Import as new copy. The modal has a copyable prompt to give Claude when designing a trip.
- **Menu photos (2026-10-24, ver 4):** the stop modal has a "Menu photos" section (existing stops only; not on locked trips). Photos are resized in the browser (max 1600px, JPEG 0.8) and uploaded to a Drive folder `VacationGuru Menus` (found or created by name; id kept in `D.menuFolderId` in the data file). Stored as `stop.menuImages = [{ fileId, name }]`. A "🍽️ Menu (n)" button on the itinerary row opens a gallery; tap a photo for full size. Remove = Drive trash. Each photo is also kept in the device's Cache API (`vg-menus`) so menus open with no signal on the device that uploaded or viewed them; other devices need Drive connected the first time. Skeleton copies start with no menu photos (so a trash on one trip can't break another).
- **Food tab (2026-10-24):** its own tab between Itinerary and Bookings, listing every Dining stop grouped by day (time, notes, link, linked bookings, spend, rating, menu button). It is a view over the same stop objects, so an edit in the Food tab or the Itinerary shows in both. "+ Food place" asks for a day, then opens the stop form preset to Dining.
- **PDF menus (2026-10-24):** the Menus section accepts photos and PDFs together. PDFs upload unchanged (max 20 MB), show as "📄 Open PDF" in the gallery (opens in a new tab), and are cached per device like photos. `menuImages` entries now carry `mime`; older entries without it are treated as JPEG.
- **Cross-tab links (2026-10-24, data v3):** one booked/paid flag is shown everywhere. A checklist item can link to a booking (`item.bookingId`): ticking it marks the booking booked/paid and ticking the booking ticks the item (the item's checkbox does not change a locked trip's bookings). Stops linked to an unbooked booking show "⚠ not booked yet" in the Itinerary and Food tabs. Each booking lists where it is used (stops and checklist items). A linked item whose state disagrees with its booking shows an amber "Booking is marked paid / not paid" badge until either is toggled. Deleting a booking removes its links. Summary has a Progress card (bookings booked, checklist done, food places with a menu). Import JSON checklist items accept an optional `booking` (words from a booking title). `migrate3()` linked existing "Pay X" items to bookings without changing any flags.
- **Safe two-way sync (2026-10-28, ver 5):** one `syncNow()` routine reads Drive, compares `updatedAt` with the last agreed version (`vacationguru-sync-meta`), and adopts, pushes or does nothing. Untouched seed data (`D.seed`, `updatedAt: 0`) never pushes and always loses to Drive (an edited seed is saved to backup first). If both sides changed, the newer wins and the other is kept in `vacationguru-backup` (header link "↩ Backup" restores it). Runs after every save, on open, every 45 s while visible, on tab focus and when back online. After the first connect, the next tap anywhere re-opens sign-in (browsers only allow it from a tap). A banner warns about in-app browsers (WhatsApp, Gmail…) where Google blocks sign-in. Scope is now full `drive` so Caroline's account can open the file Marvin shares; the file id is found by name (newest first) and remembered.
- **Export PDF (2026-10-28, ver 5):** "⬇ Export PDF" button in every trip header (also locked trips). Builds a text PDF in the browser with jsPDF 2.5.1 + jspdf-autotable 3.8.2 loaded from cdnjs on first use (preloaded when a trip opens; needs internet once). Contents: header, one table per day (time, type, stop, details incl. linked booking refs, seats and NOT BOOKED flags), bookings table with total, checklist. Built-in fonts are Latin-1 only, so emoji are dropped and arrows become `->` (`pdfTxt`). On phones it opens the share sheet (Save to Files, WhatsApp, Mail), otherwise downloads `<trip-name>.pdf`.
- **Icon:** suitcase glyph in the ecosystem icon set (`marvyeo/LifePlan/design/make_icons.py`).

## 2. Current Facts (updated 2026-10-22)

| Item | Status |
|---|---|
| GitHub repo | ✅ `marvyeo/VacationGuru` created 2026-10-22. Holds `PROJECT_STATE.md` and `CLAUDE.md` only; **no app code yet**. |
| GitHub Pages | ✅ Enabled by Marvin 2026-10-23. Site will 404 until `index.html` is committed. |
| Google OAuth | ✅ Marked done by Marvin 2026-10-23 (To-Do item 2). Not yet tested, since there is no app to sign in from. |
| Drive file | ❌ No `vacationguru-data.json` in Drive. The app has never been signed into or saved. |
| Code | ✅ `index.html` committed 2026-10-23 (uploaded by Marvin). Single file, ~33 KB. Pages should serve it at `https://marvyeo.github.io/VacationGuru/`. |
| `upcoming[]` | ✅ Already in the code: `computeUpcoming()` writes `trip` and `due-date` items at the root of `vacationguru-data.json` on every save. Untested against Life Plan. |
| Data version | ✅ v3 (2026-10-24; v2 on 2026-10-23). `migrate()` upgrades v1 data from localStorage or Drive on load, once. Perth stops were auto-linked to bookings by name. Not yet filled in for Perth: booking references, seats, pax for hotels/car/attractions, end times for stops, checklist due dates. |
| Perth full itinerary | ✅ `imports/perth-2026.json` generated 2026-10-23 from `Perth_2026_V1.pdf` (7 days, 100 stops, 18 linked to bookings). ⏳ Marvin still needs to run Import in the app. Note: the PDF has no Quest stay on 4–5 Nov (Adina all the way to 5 Nov), but the Bookings list does. Checklist item left open. |
| Menu photos | ✅ Built 2026-10-24. Logic tested in Node with a mocked Drive (folder find/create, upload body, stale-folder retry, trash, 401). ❌ **Not yet tried in a real browser or against real Drive.** Image resize, the file picker on iPhone, and the offline cache are untested. |
| Food tab and PDF menus | ✅ Built 2026-10-24. Food tab rendered in Node against `imports/perth-2026.json` (16 dining stops → 16 rows). PDF upload tested with mocked Drive. ❌ Not in a browser. ❌ Drive's multipart upload is documented for files up to ~5 MB; larger PDFs (up to the 20 MB cap) are untested and may need the resumable upload API. |
| Cross-tab links | ✅ Built 2026-10-24; migration, two-way sync, locked-trip guard and import linking tested in Node. ❌ Not in a browser. Perth: the seed marks every booking booked, but the checklist items "Pay Rottnest ferry" and "Pay Jewel Cave" are unticked, so both show the amber mismatch badge until Marvin ticks one side. Marvin should decide which is true (unpaid in the Klook app?). |
| Multi-device sync | ✅ Built 2026-10-28 (ver 5). Merge logic tested in Node against a mock Drive (11 cases: fresh phone, seed edits, conflicts both ways, shared file, stale id, backup restore). ❌ Not in a real browser or with a second Google account yet. Cause of the phone bug: the phone never connected to Drive, so it showed the seed; and a fresh seed stamped "now" would have overwritten Drive. |
| PDF export | ✅ Built 2026-10-28. Generated from the Perth import in Node (7 days, 100 stops, 4 pages) and checked visually. ❌ Not tried in a real browser; the iPhone share-sheet path (it can fall back to a plain download if Safari drops the tap after the library loads) is untested. |
| Caroline access | ⏳ Needs Marvin: (1) Google Cloud Console → OAuth consent screen: add scope `.../auth/drive` and add Caroline's Gmail as a test user; (2) share `vacationguru-data.json` AND the `VacationGuru Menus` folder with her as Editor (Claude can do this via Drive once given her address); (3) she opens the app in Safari/Chrome and taps Connect Drive. Testing-mode consent expires after 7 days, so she may be asked to sign in again. |
| Perth import | ✅ Marvin confirmed `imports/perth-2026.json` was imported into the app (2026-10-24). |
| Ecosystem lessons | ✅ Already in the code: direct Drive `fetch()`, `res.ok` checks, 401 reconnect. ❌ Not yet: PWA files (`manifest.json`, `sw.js`, icons), iPhone redirect sign-in fallback, multi-device merge. |
| Seed data | Perth & Margaret River, 2–8 Nov 2026, 7 days. Travellers: Marvin, Caroline, Cobae (toddler) and a guest adult. |

**Conclusion:** VacationGuru was built but never deployed or used. The Perth trip was planned and paid outside the app. **Never modify the "Caroline & Marvin Life Planner" Google Sheet**; read it only if Marvin asks.

## 3. Perth Trip: Seed Data vs Actual Bookings

As of 16 Oct 2026, the seed data in `index.html` doesn't match the Life Planner sheet. It was never reconciled.

| Booking | In VacationGuru seed | Actual (Life Planner sheet) |
|---|---|---|
| SIA flights (Marvin + Cobae) | SQ213 out 2 Nov 7:20am, SQ226 back 8 Nov 1:50pm, no cost | $1,408 |
| SIA flights (Caroline) | not split out | $908.80 |
| Adina Barrack Plaza, 2–5 Nov | merged with the 7–8 Nov stay | $1,371.41 via Trip.com, deduction 30 Oct |
| **Quest, 4–5 Nov** | ❌ missing | Trip.com, amount not recorded |
| Margaret River Resort, 5–7 Nov | not confirmed | $752.25 via Trip.com, paid 4 Jul |
| Adina Barrack Plaza, 7–8 Nov | merged | $499.37 via Trip.com, deduction 4 Oct |
| Car rental (Ace, Redcliffe; shuttle from T1) | not confirmed | $488.90 via Klook, paid 9 Aug |
| **Perth Zoo** | ❌ missing | $144.02, paid 28 Sep |
| **Rottnest Bus** | ❌ missing | $131.56, paid 28 Sep |
| Rottnest Ferry | not confirmed | via Klook, **not yet paid** |
| Jewel Cave | not confirmed | via Klook, **not yet paid** |

**Itinerary notes to keep:**
- Jewel Cave allows a front carrier only (no prams).
- Leave the hotel by 9:45am on 8 Nov to return the rental car by 10:30am.
- Check which Leeuwin Estate area wineries are toddler-friendly.

**Update 2026-10-20:** Marvin confirmed all Perth bookings and payments are settled outside the app. The table above is kept only as reference for entering actuals later.

## 4. Decision for the Hold Period

Don't deploy VacationGuru for Perth; the trip is fully settled outside the app. **Recommended:** after the trip, enter Perth's actuals once (costs, ratings) and lock it, so it becomes the first skeleton for the next trip.

## 5. To-Do When Resuming

1. ✅ DONE 2026-10-23: `index.html` committed.
2. ✅ DONE 2026-10-23: GitHub Pages enabled and Google Cloud OAuth set up. Still to verify once `index.html` is live: sign-in works from `https://marvyeo.github.io/VacationGuru/`.
3. Apply the ecosystem lessons learned since the build:
   - Use direct `fetch()` calls to the Drive REST API (no `gapi.client.drive` or `discoveryDocs`).
   - Check `res.ok` on every save and load (TV World had a silent-save bug).
   - Handle the 401 token-expiry reconnect.
   - PWA: `manifest.json` with `"id": "/VacationGuru/"` (never `"./"`), network-first `sw.js`, ecosystem icons, iPhone redirect sign-in fallback.
   - Optional: multi-device merge, as in TV World.
4. Perth seed data is already reconciled with Section 3 in the code (Quest, Zoo and Rottnest bus added, Adina stays split). Still to do after the trip: enter the missing amounts (Quest, Rottnest ferry, Jewel Cave), mark the trip Locked, and do the ratings pass.
5. ✅ `upcoming[]` is built (see Section 2). Remaining: confirm Life Plan reads it. Spec for reference:
   - `type: "trip"` for departure dates (priority high within 7 days).
   - `type: "due-date"` for booking deadlines and unpaid items.
   - Recompute on every save, at the root of `vacationguru-data.json`.
5a. **Before 2 Nov, Marvin tries the live app once** (Drive sign-in, Import `imports/perth-2026.json`, add a menu photo to a dining stop, reopen it with Wi-Fi off). Report anything odd to a Sonnet session. Menu photos are untested in a browser.
5b. ✅ DONE 2026-10-24: "sync across the tabs" built as cross-tab consistency (Marvin chose this meaning; two-browser-tab/device live sync was NOT built, the app still syncs to Drive on save or Sync now).
6. **Model:** A and B are Sonnet jobs (M). C is Sonnet (M); only switch to Opus if the old code needs a large rework.

## 6. Change Log

- **ver 5 (2026-10-28, later):** Export PDF button (see Features). Tested in Node only.
- **ver 5 (2026-10-28):** Phone showed old seed data because it never synced. Rewrote sync (see Features, safe two-way sync), full `drive` scope for a shared file, in-app browser warning. Tested in Node with mock Drive only. Next chat name bumped to ver 6.
- **ver 1 (2026-10-20):** Initial state file. Recorded that the app was never deployed, the Perth booking reference, and the hold decision. Perth confirmed settled.
- **ver 2 (2026-10-22):** moved into the new `marvyeo/VacationGuru` repo; added Session Rules and `CLAUDE.md`; PWA checklist added to the resume steps.
- **ver 3 (2026-10-23):** Marvin enabled GitHub Pages and finished the Google OAuth setup. Recorded both. `index.html` is still not in the repo (it was never found), so it is the next blocker. Next chat name bumped to ver 4.
- **ver 3 (2026-10-23, later):** `index.html` uploaded by Marvin and committed. It already has the reconciled Perth seed, `upcoming[]`, direct Drive fetch, `res.ok` checks and 401 reconnect. PWA files and the iPhone sign-in fallback are still missing.
- **ver 3 (2026-10-23, later still):** Booking details (category, reference, booked via, pax, carrier, seats), start and end times on stops, stop-to-booking links, and a Checklist tab. Added `migrate()` so existing Drive data upgrades without loss. Tested in Node against the seed only, not against Marvin's saved Drive file or in a browser.
- **ver 3 (2026-10-23, checkpoint 1 of the second batch):** Timeline layout with durations, on-the-day spend prompt, shopping and rest types, day titles, and a trip Import function with a Claude prompt. Generated `imports/perth-2026.json` from the PDF. Tested in Node (parsing, linking, durations, error messages), not in a browser. Menu images are next (see RESUME HERE).
- **ver 4 (2026-10-24, checkpoint 2 of 2):** Menu photos: upload from the stop modal to a `VacationGuru Menus` Drive folder, in-browser resize, gallery button on the itinerary row, remove to Drive trash, per-device offline cache. Skeleton copies drop menu links. The import "Update itinerary" warning now mentions menu photo links. Removed the RESUME HERE block (step done). Tested in Node with mocked Drive only, not in a browser. Next chat name bumped to ver 5.
- **ver 4 (2026-10-24, later):** Food tab (view over Dining stops, with "+ Food place") and PDF support for menus. Marvin confirmed the Perth import is done. Open question for Marvin: what "sync across the tabs" means (see To-Do 5b). Tested in Node only.
- **ver 4 (2026-10-24, later still):** Cross-tab consistency (Marvin's pick for "sync across tabs"): checklist↔booking link with two-way ticking, "not booked yet" warnings on stops, "Used in" on bookings, Progress card on Summary, import `booking` field, data v3 migration. Next chat name stays ver 5. Tested in Node only.
