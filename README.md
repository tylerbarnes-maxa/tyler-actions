# Tyler Actions v6.3

v6.3 replaces the hourly winter background with the approved day/night theme. The whole main pane now follows UK time: dark before dawn, lightening through sunrise, light all day, darkening through sunset. Tasks, sync, layout and every screen work as before.

## What's new in v6.3

### Scenery

- Six full-colour scenes in an `assets` folder next to `index.html`: night, dawn, morning, noon, sunset, twilight. They are the pack's files, unmodified.
- The app blends between neighbouring scenes by the minute, on the pack's schedule: night until 06:00, dawn at 08:00, morning at 10:00, noon at 12:00, sunset at 15:00, twilight at 16:00, night from 18:00.
- The 15% graduated wash is applied at runtime in the current canvas colour, so it is pale by day and dark slate at night. The picture contributes 1.2% at the top, 3.3% at 37% of the height, 9.75% at 66% and 15% at the bottom.
- The 24 pre-faded images and the extra pale overlay from v6.2 are gone. Nothing is stacked.
- Scenes sit behind the content, `cover`, anchored centre bottom, and take no clicks.

### Colours

- Canvas, cards, the add box, borders, headings, text, metadata, sheets and menus all come from the pack's `paletteAt()` calculation, recalculated once a minute. The maths was checked against the pack's own script for all 1,440 minutes and matches exactly.
- Cards stay opaque: dark slate with light text at night, white with dark text by day, mid tones in between.
- The sidebar stays black and the yellow is unchanged.
- This app has more colours than the reference palette covers (secondary text, red, amber and green labels, the pinned star, pale-yellow panels). Each is chosen the same way the reference picks its text: by contrast against the surface it sits on, at least 4.5:1.
- The correct colours are set in the page head, before anything is drawn, so a night launch never flashes white.

### Timing

- Uses the actual hour and minute in Europe/London, whatever the device's time zone, including both clock changes.
- Updates on each minute boundary while the app is visible. Only the theme variables and the background layers change; the board is not rebuilt.
- Stops while hidden. Recalculates at once on return, on window focus and after sleep. One timer only.
- No animation runs between minutes. The largest one-minute colour step is 3 of 255. The scenery fades in once on launch over 0.6s, or not at all with "reduce motion" on.

### Loading

- The scenes needed now are fetched and decoded before they show, then the next one in advance. Each of the six is fetched once and kept.
- While a scene is loading, the last complete scene stays. If nothing has loaded, the pane is the current canvas colour.
- Failed loads retry after 1, 2, 4 and 8 minutes, then every 15, and straight away when the connection returns.

## Things that changed with it

- **Manual Dark mode is removed.** The Light/Dark/System chips in Settings and the Dark mode item in the More menu are gone, because the appearance now follows the time of day. A previously saved choice is ignored.
- **Small text is darker by day than in v6.1.** Metadata under each task uses the reference's muted colour (a slate grey, about 7:1 on white). In v6.1 it was a paler grey at about 3.3:1.
- **"Pinned" and "Due today" labels are a darker gold and amber by day**, so they reach 4.5:1 on white. At night they are the bright versions. The yellow card edge, buttons and branding are untouched.
- **Labels that sit straight on the scenery have a two-pixel outline in the canvas colour** behind the letters (day names, people's names, column headings). You can't see it on flat canvas. It keeps the picture from showing through right behind the text.
- **Muted labels on the canvas switch to the heading colour slightly earlier than the reference does** (28 minutes a day), because they have scenery behind them. They are never weaker than the reference.

## Install

1. In the current app: Settings, Back up now.
2. In the GitHub repo choose Add file, Upload files, then drag in `index.html` and the whole `assets` folder together. Leave `sw.js` where it is.
3. Reload on each device. Settings should say v6.3.
4. Optional: delete the old `backgrounds` folder from the repo. Nothing uses it now.

If `index.html` goes up without the `assets` folder the theme still works and the pane is plain canvas colour.

Size: the six scenes total 3.5 MB (450 to 670 KB each). A launch fetches two or three of them.

## Tested

Headless Chromium with a faked clock, seeded data and mocked Supabase. 219 checks passed with no page errors.

- **Six named times:** at 06:00, 08:00, 12:00, 15:00, 16:00 and 18:00 the scene, canvas, card, border, text, metadata and heading colours equal the pack's `hourly-theme-colours.json`. Compared side by side with the pack's reference page.
- **Minute by minute:** 45 consecutive minutes through dawn (07:30 to 08:14), each showing that minute's reference colours and blend. Background screenshots one minute either side of every scene change differ by under half a level on average.
- **Wash:** rendered pixels compared with the scene files at four times, including two 50/50 blends. Measured share of the picture is 1.2% at the top and 15% at the bottom. No pale overlay remains at night.
- **Clock:** every ten minutes of 2026 against the UK time zone database with the browser on Tokyo time, midnight rollover, and both clock changes run live.
- **Hidden and asleep:** seven hours hidden (no work done), then visible; a 3h10 sleep; focus the next morning; page restore. Eighteen wake events still leave one timer.
- **Loading:** each scene fetched once over 24 hours, a slow image at launch, a slow second image (last scene stays), every image failing (dark canvas at night, retries back off, recovery).
- **Layout:** positions, sizes and type compared with v6.1 at four screen sizes on four screens, by day, at night and at dawn. No differences.
- **Interactions at night and at dawn:** quick add, tick and undo toast, expand, drag between columns, keyboard selection, People, chase sheet, Settings, phone More menu.
- **Contrast:** every text node on every screen and sheet (board, open cards, quick-add preview, People, Done, Review, Settings, search, capture, chase, picker, move, More, toast), desktop and phone, against its own surface. Every 10 minutes all day and every minute from 06:00 to 10:00 and 14:00 to 18:00: 941,875 checks, lowest 4.50:1.

## Remaining issues

- **Labels on the scenery around the mid-grey moment.** Measured against the real pixels behind the letters, 98% of those pixels meet 4.5:1 at every minute tested (lowest 4.54). Single pixels dip to 4.24:1 in 10 of the 265 minutes tested, around 07:41 to 07:53, 14:54 and 15:33 to 15:41. That is the point where the canvas is mid-grey and even pure black or white only reaches 4.6:1 on it, so there is no stronger colour to switch to.
- **Text flips between light and dark once at dawn and once at dusk.** Card text turns dark at 07:35 and heading text at 07:41; in the evening headings turn light at 15:42 and card text at 15:49. Each flip happens in a single minute. This comes from the reference's readable-colour rule and is what keeps text above 4.5:1 through the mid-greys.
- **Sidebar hint text** (the keyboard shortcuts at the bottom) is the same faint grey on black as before, below 4.5:1. The sidebar was left alone as asked.
- **The Focus button** is black with yellow text at night, as in the reference, so the pill itself barely stands out from the dark canvas. The text is clear.
- **Old browsers:** the label outline needs Chromium 123 or later, Safari or Firefox. Older Chromium gets a thinner one-pixel version, which was not tested.

Not tested: the live GitHub Pages site, a physical phone, the installed Windows app, a real sleep and wake, Safari and Firefox. The hosted reference link was not opened; the comparison used the copy of the reference page inside the pack. The full v6.1 suite of 118 checks was not re-run, only the interaction tests above.

---

# v6.2.2 notes

v6.2.2 takes the winter background down to about 15% strength. Only `index.html` changed. The image files are the same.

## What's new in v6.2.2

- The single wash over the images is now `rgba(246, 247, 248, 0.4643)`. It replaces the 0.286 wash from v6.2.1. There is still only one overlay.
- The images carry 28% strength, so the wash leaves about 15% (28 x 0.5357). Measured on screen against the supplied midday image: 15.1%.
- Cards, text, controls, hourly switching and the crossfade are unchanged.
- The faintest grey label on the picture now measures 5.7:1 at its worst spot.

## Install

Upload this `index.html` to the GitHub repo in place of the current one. Leave `sw.js` and the `backgrounds` folder as they are. Settings should say v6.2.2 after a reload.

## Tested

157 checks passed in headless Chromium: the v6.2.1 set re-run with the new value, plus a check that only one overlay exists and the old one is gone.

Not tested: the live GitHub Pages site, a physical phone or the installed Windows app.

---

# v6.2.1 notes

v6.2.1 softens the winter background from 28% to about 20% strength. Only `index.html` changed. The image files are the same.

## What's new in v6.2.1

- A uniform wash, `rgba(246, 247, 248, 0.286)`, sits above both image layers and below every card, label and control. It takes no clicks.
- The images already carry the 28% treatment, so the wash leaves about 20% (28 x 0.714). Measured on screen against the supplied midday image: 20.3%.
- No opacity is applied to the app or its content. Cards are still pure white.
- Hourly switching and the 650ms crossfade run underneath the wash as before.
- Text on the picture is a little easier to read as a result: the faintest grey label now measures 5.1:1 at its worst spot (4.2:1 in v6.2).

## Install

Upload this `index.html` to the GitHub repo in place of the current one. The `backgrounds` folder has to be in the repo too, at `backgrounds/winter-00.webp` to `backgrounds/winter-23.webp`. Settings should say v6.2.1 after a reload.

## Tested

155 checks passed in headless Chromium: the 145 from v6.2 re-run, plus overlay colour, stacking order, click-through, card colour, a pixel comparison of the rendered background against the supplied image with the wash applied, and the crossfade under the overlay.

Not tested: the live GitHub Pages site, a physical phone or the installed Windows app.

---

# v6.2 notes

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
