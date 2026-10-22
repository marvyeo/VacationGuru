# VacationGuru — Project State

> **Next chat name: `VacationGuru ver 3`**
> Claude: your FIRST reply in a new chat must start with "Rename this chat to: **VacationGuru ver 3**" (you can't rename chats yourself). In your first commit this session, bump the number here by 1.

**State version:** 2 (ver 1 written 2026-10-20 in "Life Plan Roadmap ver 2"; moved into the repo 2026-10-22 by "Life Plan Roadmap ver 3")
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
- **Icon:** suitcase glyph in the ecosystem icon set (`marvyeo/LifePlan/design/make_icons.py`).

## 2. Current Facts (updated 2026-10-22)

| Item | Status |
|---|---|
| GitHub repo | ✅ `marvyeo/VacationGuru` created 2026-10-22. Holds this file only; **no app code yet**. Pages not enabled. |
| Drive file | ❌ No `vacationguru-data.json` in Drive. The app has never been signed into or saved. |
| Code | `index.html` was delivered as a download in the original chat ("VacationGuru", last updated 2026-10-16). It lives only in that chat or on Marvin's computer. |
| `upcoming[]` | Not built. |
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

1. Locate `index.html` (Marvin's computer, or the original chat's download) and commit it here.
2. Enable GitHub Pages, and add `https://marvyeo.github.io/VacationGuru/` to Authorized redirect URIs in Google Cloud (the JS origin `https://marvyeo.github.io` is already registered).
3. Apply the ecosystem lessons learned since the build:
   - Use direct `fetch()` calls to the Drive REST API (no `gapi.client.drive` or `discoveryDocs`).
   - Check `res.ok` on every save and load (TV World had a silent-save bug).
   - Handle the 401 token-expiry reconnect.
   - PWA: `manifest.json` with `"id": "/VacationGuru/"` (never `"./"`), network-first `sw.js`, ecosystem icons, iPhone redirect sign-in fallback.
   - Optional: multi-device merge, as in TV World.
4. Reconcile the Perth seed data with Section 3, mark the trip Locked, and do the ratings pass.
5. Add `upcoming[]` per the ecosystem envelope:
   - `type: "trip"` for departure dates (priority high within 7 days).
   - `type: "due-date"` for booking deadlines and unpaid items.
   - Recompute on every save, at the root of `vacationguru-data.json`.
6. **Model:** A and B are Sonnet jobs (M). C is Sonnet (M); only switch to Opus if the old code needs a large rework.

## 6. Change Log

- **ver 1 (2026-10-20):** Initial state file. Recorded that the app was never deployed, the Perth booking reference, and the hold decision. Perth confirmed settled.
- **ver 2 (2026-10-22):** moved into the new `marvyeo/VacationGuru` repo; added Session Rules and `CLAUDE.md`; PWA checklist added to the resume steps.
