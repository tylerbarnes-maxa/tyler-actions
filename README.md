# Tyler Actions v6.1

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
