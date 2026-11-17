# Tyler Actions v6.2

v6.2 adds the hourly winter background behind the main pane. Tasks, sync, layout and every screen from v6.1 work as before.

## What's new in v6.2

### Winter background

- The 24 WebP images sit in a `backgrounds` folder next to `index.html`, named `winter-00.webp` to `winter-23.webp`.
- The app picks the image for the current hour in UK time (Europe/London, so GMT and BST are both handled) whatever time zone the device is set to.
- Images are shown as supplied: full opacity, `cover`, anchored centre bottom so the railing stays along the bottom edge. No overlay, blur or filter is added.
- The picture fills the main pane on every screen (board, People, Done, Review, Settings). The sidebar is untouched. On the phone it stops at the top of the bottom bar.
- The right image shows on launch with no fade. The next hour's image is fetched in advance and fades in over 650ms on the hour. With "reduce motion" switched on in the device settings it swaps with no fade.
- The time is rechecked when the window regains focus, becomes visible or comes back from sleep, and at least once a minute while open. Nothing runs while the app is hidden.
- If an image fails to load the current one stays. If nothing has loaded yet the pane is plain pale grey (#F6F7F8). Failed loads retry after 1, 2, 4 and 8 minutes, then every 15.
- The background takes no clicks and doesn't change the size or scrolling of anything. Column and card positions are identical to v6.1.

### Two small things that changed with it

- The top bar on the laptop (the strip holding the add box) is now transparent so the picture runs to the top of the pane. It was flat grey.
- Small pale-grey labels that sit straight on the picture (column counts, empty day names, "Who owes you what?" style subtitles) now use the app's existing darker grey. Over the railing the pale grey measured 1.65:1 contrast, which was too faint. They now measure 4.2:1 or better at the worst spot. Text inside cards is unchanged.

### Dark mode

Nothing changes colour at night. The images are pale at every hour, so the light interface is used all day.

The app's existing manual Dark mode (More, Dark mode) is still there and shows its own dark background with no picture, because pale images behind light text would be unreadable. If the device is set to dark and the app theme has never been chosen, the app follows the device and you won't see the picture until you pick Light mode.

## Install

1. In the current app: Settings, Back up now.
2. In the GitHub repo choose Add file, Upload files, then drag in `index.html` and the whole `backgrounds` folder together. GitHub keeps the folder. Leave `sw.js` where it is.
3. Reload on each device. Settings should say v6.2.

If `index.html` goes up without the folder the app still works and shows plain pale grey.

Size: the folder is 3.8 MB in total. Each image is 130 to 240 KB and only the current and next hour are fetched. The images are not stored for offline use, so with no connection and nothing cached the pane is pale grey.

## Tested

Headless Chromium with a faked clock, seeded data and mocked Supabase. 145 checks passed with no page errors.

- Hour selection: every half hour of 2026 against the UK time zone database, with the browser set to Tokyo time. Includes both clock changes (01:00 skipped in March, 01:00 repeated in October).
- On the hour: 10:59:59 to 11:00, crossfade measured mid-way (old image fully visible underneath, new one fading in), old layer cleared after, three hours in a row.
- Midnight: 23:59:59 to 00:00, preload is `winter-00`.
- Sleep: clock jumped 5 hours with the lid closed; clock moved with no timer firing then window focus; hidden then visible; page restored from the back/forward cache.
- Loading: slow image held back (current stays, no blank, no duplicate request), failed image at the hour (current stays, retries back off, recovers), every image failing (pale grey, board still works).
- Reduced motion, start/stop cleanup of timers and listeners, locked device (nothing fetched until unlocked).
- Layout: positions and sizes of the sidebar, top bar, columns, headings and cards compared with v6.1 at 1920, 1440, 1280, 1000 and 390 wide on four screens. No differences.
- Still working: quick add, tick done, expand a card, drag between columns, People, chase sheet, quick-add preview above the board. Cards solid white, sidebar black, yellow unchanged.
- Readability: contrast of all text on the picture measured against the darkest pixel behind it at 00, 08, 10, 12, 16 and 17 hours.

Not tested: the live GitHub Pages site, a physical phone or the installed Windows app, a real sleep and wake, and Safari or Firefox. The full v6.1 suite of 118 checks was not re-run, only the smoke test above.

---

# v6.1 notes


v6.1 adds the whole Maxa team and a chaser email for a single action. Everything from v6.0 is unchanged.

## What's new in v6.1

### The whole company, with avatars

All 35 caricatures are built into the app with each person's work email, so anyone at Maxa can be given an action or chased without setting them up first.

- Team is still Braden, Callum, Jacob and Sheldon. Everyone else counts as Other. Anyone you'd previously saved as Team is moved to Other once, on first load.
- Avatars show on the Waiting column, in People, in the picker, and beside the name on a card.
- The person picker (Waiting on..., and More options when adding) shows the team and your recent people. Tap "Everyone at Maxa" for the rest, or start typing a name.
- Typing or dictating a first name works for all of them: "Ask Kentley for the stock report Friday". Full names work too. The two Andrews and two Jameses are "Andrew B", "Andrew E", "James B" and "James D".
- "Chase Ian at CP Hire about the PO" creates an outside contact called "Ian (CP Hire)" so it can't be mistaken for Ian Stedman. This applies to any colleague's first name followed by "at" or "from" and a company.
- Colleagues only appear in People once they have something open, so the list stays short.
- Emails came from the Maxa directory on 12 Nov 2026. Admire and Ion have no work mailbox there, so they have an avatar but no email. Add one from People if needed.

### Chaser email for one action

On any Waiting card, Chase (or the envelope on hover) opens a chaser for that one action.

- The draft is written the way you write: time-of-day greeting, short lines ending in commas, "Thanks," to close, no signature line.
- The wording is drawn from a pool and never repeats the last pick. Reword gives another version. A second chase on the same action reads as a follow-up ("Sorry to chase again").
- Subject and message are both editable before sending.
- Email opens the draft in Outlook addressed to them. Nothing is sent until you press send there.
- If they owe you more than one thing, switch to "All I'm waiting on" for a single round-up email.
- Sending logs the chase and sets the next chase date, as before.

## Install

1. In the current app: Settings, Back up now.
2. Upload this `index.html` to the GitHub repo in place of the current one. Leave `sw.js` where it is.
3. Reload on each device. Settings should say v6.1.

One thing to know before uploading: the GitHub repo is public, so the page source will contain 35 first names, caricatures and work email addresses. The four team emails were already there.

## Tested

Headless Chromium with seeded data and mocked Supabase: 118 checks passed with no page errors, covering everything listed for v6.0 plus the staff directory, avatars, name parsing, pickers, per-action chasers and wording variation.

Not tested: the live Supabase project, a physical phone, the handover from the Email button into Outlook, real Wispr Flow dictation, swipe gestures, and weekend dates.

---

## v6.0 notes

v5.2 was built to keep Today short. It capped the list, let older items slip out of view and put the rest behind a Later tab. That made the list hard to see, so it stopped being the place to work from. v6 keeps the v5 engine and replaces the screens with one board that shows every open action.

### What changed

### One board instead of Today and Later

Four columns, all visible at once on a laptop:

- Today: everything dated today or earlier, pinned items first, late deadlines in red.
- Coming up: the next five working days, one heading per day, so you can see the shape of the week.
- Later: actions with no date at the top, then anything scheduled further out.
- Waiting: grouped by person, with a Chase button on each. The button is yellow when a chase is due.

Nothing is capped, slipped or hidden at weekends any more. If it's open, it's on the board. On the phone the same four sections stack in one scroll, with four pills at the top to jump between them. Tap a section heading to fold it.

### Adding an action

On the laptop there's a bar across the top of every screen. Press N, type or dictate with Wispr Flow, press Enter. The bar stays focused for the next one. As you type it shows what it understood and which column the action will land in.

- "Chase Sheldon about the Speedy rebate Friday" goes to Waiting on Sheldon, chase Friday.
- "Send the KDM letter by Tuesday" goes to Tuesday as a deadline.
- No date mentioned means it lands in No date, top of the Later column.
- Paste a list and each line becomes an action. Bullets and numbering are stripped.
- Dictate several sentences and it offers to split them into separate actions.

On the phone the + button opens the same single-field capture as before.

### Moving things

- Drag a card to another column, a day, or a person. Dropping on a person marks it Waiting on them.
- Hover a card for pin, move, waiting and edit.
- Click a card for Done, Tomorrow, Waiting and More.
- Keyboard: arrows to select, then X done, T today, M tomorrow, W waiting, S pin, E edit, D move. The letters only work after you've selected a card with the arrows.
- Every change still has Undo in the toast.

### Removed

- The cap on Today, the Top 3 suggestions and "Start here".
- Slipped items and the sorter prompts.
- Weekend hiding.
- The Later tab (now a column) and the sidebar Capture button (now the bar).
- The wrap-up prompt.

### Kept as it was

People, chase-all messages, Focus, Done, Review, Settings, backups, password lock, Supabase sync, recurring actions, the email-to-task inbox.

### Compatibility

- Data is unchanged. Same storage keys, same task fields, same Supabase tables and sync logic. v5.2 and v6 can read each other's data.
- A one-off backup called "Before v6" is taken on first load. Restore it from Settings, Backups.
- Actions created by the email parser arrive in the right column and flash yellow when they land. They carry a small envelope mark.


### v6.0 testing

Headless Chromium, 31 seeded actions in both v5 and email-parser shapes, mocked Supabase. 74 checks passed with no page errors: layout at 1920, 1600, 1366, 960 and 390 wide; the add bar with dictated phrases, deadlines, pasted lists and sentence splitting; drag between every column; hover and keyboard actions; undo; search; chase sheet; focus; dark mode; lock screen on a fresh device; first sync onto an empty device; an emailed action arriving through sync.

Not tested: the live Supabase project, a physical phone, real Wispr Flow dictation, swipe gestures (code unchanged from v5.2), and weekend dates.
