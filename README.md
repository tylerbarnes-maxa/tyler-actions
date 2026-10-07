# Tyler Actions v5.2

This release is a usability pass on v5.1. The model is unchanged (Today / People / Later, Mine / Waiting / Done, Top 3, chase-all, soft dates vs real deadlines, undo, Supabase sync). This release removes, merges and caps rather than adds.

## What changed

### Removed

- Morning banner. It is replaced by "Start here" on the first Top 3 card before 11:00.
- "I'm with [Name]" conversation overlay. It is replaced by a small **1:1 mode** chip that enlarges the expanded person card.
- Second "Remind me again" chase sheet, and the "Sent — log chase" step.
- Mobile top-bar ⋯. The bottom-nav **More** is the only one.
- "Show more" on Today.
- Eight-button expanded cards.
- The Parked / Unplanned / "Later — no reminder" options in capture and edit.

### Merged

- Pinning is always **Top 3** (★). **Focus** is only the one-at-a-time session.
- Someone owes you → **Waiting on [Name]**. Yours, involving them → **To raise with [Name]**.
- Undated and parked items → **No date**, a group inside Later. Parked records keep their data field.
- Snooze, dates, Top 3, Edit and Drop → one **More…** sheet per item.

### Changed

- **Today is finite.** It shows Top 3, Chase (max 3 people, then "+N more in People"), and at most 5 other items.
  - Order: deadlines, then items dated today, then items linked to a person, then most recently scheduled.
  - The count pill always equals what's visible.
- **Top 3 suggestions** only come from deadlines, items dated today and legacy High priority. They never come from old items. They are shown lighter, with ★ to keep, and only when at least 2 good candidates exist.
- **Expanded cards have four buttons, 44px tall, on one line at 360px.**
  - Mine: Done · Tomorrow · Waiting · More…
  - Waiting: Got it · Chase · My move · More…
  - Swipe left: Tomorrow / Waiting / More (Waiting items: Chase / My move / More). Swipe right is still Done.
- **Chase takes two taps.** One sheet has the message, a "Next chase" picker (default 2 working days) and Email / WhatsApp-Share / Copy / Just log it.
  - Sending logs the chase immediately, with Undo.
  - A cancelled share doesn't log. Later agreed dates on items are still preserved.
- **Working days.** The Tomorrow buttons, swipe, wrap, Focus and sorter target the next working day, and read "Monday" on Friday. "tomorrow" typed in capture keeps its literal meaning.
- **Weekends.** Today shows only deadlines and Personal items, plus "Weekend — N work items wait for Monday".
- **Wrap-up** opens with "✓ N done today. M to decide." It counts visible items only, 5 at a time. The buttons are Tomorrow · Next week · Waiting · Drop. "Later" no longer sends things to No date.

### Added

- **Slipped.** An ordinary dated item that sits on Today for 2+ working days drops out of view.
  - It is derived from `due` alone: nothing is deleted or re-dated.
  - One quiet line ("4 slipped · 4 more for today — sort in 30 seconds ›") opens a one-at-a-time sorter: Tomorrow · This week · Next week · No date · Drop, and swipe right = Tomorrow.
  - Items moved 3× get "still worth doing?" with Drop highlighted.
  - The same items also appear at the top of Later.
- **Weekly No-date sort.** On Mondays, or when No date passes 10 items, one line offers a 2-minute sort: This week · Next week · Keep · Drop. It shows once per week.

## Compatibility

- **Data is unchanged.** Storage keys, task schema, Supabase tables, tombstones, merge and retry queue are all as before. There are no new task fields.
- **A new backup is taken.** `backup_pre_v52` is a one-time snapshot taken on first load. It can be restored from Settings › Backups, alongside `backup_pre_v5` and `backup_pre_v51`.
- **Don't run v4 alongside v5.x.** v4 still has the old cloud-deletion behaviour.

## Install

1. In the current app: Settings › Back up now.
2. Replace the deployed `index.html`, keep `sw.js` beside it (unchanged from v5.1), and keep the same host and path.
3. Reload. Settings should say v5.2.

## Verification

Headless Chromium was run with 30+ seeded actions, 5 people with due chases, slipped, undated, deadline and recurring items, with a mocked clock and mocked Supabase. All 21 checks passed with no page errors:

- Top 3 and the first chase are visible without scrolling at 390×844.
- Today shows ≤ 3 + 5 cards and ≤ 3 chase people, and the pill matches.
- Slipped items leave Today with no data change.
- The sorter defers and advances, and moved-3× items highlight Drop.
- Chase is logged in 2 taps with next = +2 working days, and Undo restores it.
- Expanded cards have 4 buttons, one row at 360px, ≥ 44px tall.
- Retired words are absent across Today, People, Later, capture and the More sheet.
- Morning "Start here", the evening wrap count, and Friday → "Monday" behave as specified.
- Weekend filtering works, and the Monday No-date nudge appears.
- v4 data migrates with zero loss and remote rows merge, with no remote DELETE calls.

Not verified: live Supabase, real email or share delivery, and behaviour on a physical phone.
