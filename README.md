# Tyler Actions v5.1

A refinement of the supplied V5 application, using its existing task records, contacts, recurrence, completion history, local storage and Supabase sync. The original v4 file is retained separately as a reference.

## Audit and decisions

V5 already had strong foundations: waiting actions, local parsing, additive migration, undo, recurring actions, contact sync, backups and tombstone-based deletion. These have been retained.

The remaining friction was Inbox processing, overlapping priority/focus language, a dominant timer, redundant Later/Parked choices, rigid chase reminders, and a long wrap-up. Inbox is now a compatibility route to Later; it is no longer an obligation or navigation tab. Undated and parked records remain intact.

Chase copying/sharing previously logged success even when it failed or was cancelled. This now waits for success. Opening an email draft does not count as sending it: the app offers “Sent — log chase” afterwards.

## Product model

| Area | Behaviour |
| --- | --- |
| Navigation | Today · People · + · Later · More. Secondary functions remain in More and the desktop sidebar. |
| Today | Your 3, Chase, Today. Quiet Later/Waiting counts and an evening Wrap up link. Four other actions are initially visible; more can be revealed. |
| Top 3 | Up to three pinned actions. A fourth opens a swap chooser, including when captured or edited. Pins appear first; unfilled places may show due deadlines. If nothing is pinned or due as a deadline, busy days suggest up to three actions. |
| Urgency | Deadline counts are always visible; late deadlines sort first among the remaining Today actions. Ordinary resurfacing dates never turn red. |
| People | “They owe me” and “I need to raise”, with due chases, last chase and next reminder. |
| Conversation | Expand a person and tap “I’m with [name]”. Tick off talking points, receive owed items, or open their quick actions. Updates and undo appear immediately. |
| Capture | Type and press Enter. Undated actions save directly into Later. Local parsing produces removable person/date/area/Top 3 chips; advanced fields stay under More options. |
| Dates | Ordinary dates mean “When should I see this again?”. “Actual deadline” stays in More options. Tomorrow means the next calendar day; recurrence and the two-working-day chase option skip weekends. |
| Later | Future items plus Unplanned. No Inbox triage and no required distinction between undated and parked actions. |
| Focus | One action with Done / Waiting / Next. Timer appears only when requested and resets for the next action. |
| Chase | Draft one message for all outstanding items. Choose Tomorrow / 2 working days / Friday / Next week and confirm. Future agreed dates later than the selected reminder are preserved per item. |
| Wrap | Five relevant decisions at a time. Tomorrow / Later / Waiting / Drop. Stale tasks offer Tomorrow / Later / Drop, without a Keep loophole. No automatic rescheduling. |
| Review | Late, waiting and stale counts, a small area breakdown of completions this week, and up to six stale decisions. Percentages describe completed actions, not measured time. |

## Compatibility and backups

- Storage keys and the V5 task schema are unchanged. No schema change or task deletion is needed to absorb Inbox into Later.
- Existing undated `mine` and `parked` items appear in Later → Unplanned.
- V4 migration is preserved, including context mapping and original date retention for old ordinary tasks. Actual deadlines are protected from the old fresh-start date reset.
- The permanent `backup_pre_v5` and daily backups are retained. An additional one-time `backup_pre_v51` snapshot is taken when existing local tasks first load; both permanent snapshots can be restored in Settings.
- Existing Supabase tables, tombstones, dirty rows, pull/merge, retry queues, contact records and completion history remain compatible. Do not use the old v4 app alongside this version: v4 has older cloud deletion behaviour.
- The application does not send messages automatically. Email opens the device email client; sharing opens the system share sheet.

## Install / update

1. In the existing app, use Settings → Back up now.
2. Replace its deployed `index.html` with the included `index.html`, and put `sw.js` beside it. Keep the same hostname and app path so the browser can access the existing local data.
3. Reload the app. Settings reports v5.1. Existing tasks and contacts should be present; undated items are under Later.

The HTML works on its own; the small companion worker enables offline app-shell loading when hosted over HTTPS (or localhost). Opening a downloaded file is a separate browser origin and does not automatically access the existing installation's local data. No live site has been deployed by this update.

## PWA

The previous blob service-worker registration was rejected by browsers. The included real `sw.js` registers with the app directory as its scope. It caches app navigation only, never Supabase task/contact responses. Updates use the network when available and the cached app shell when offline. External web fonts fall back to system fonts offline.

## Verification

Headless Chromium checks cover capture and natural-language examples, Today membership, the three-item cap and swapping, optional timers, conversation updates and undo, reminder choices and preserved future promises, share cancellation, recurring completion/undo, stale wrap decisions, reload persistence, retained history, and all main screens at 360px, 390px and 1280px. Extra checks cover notes retained when More options is collapsed, V4 migration and permanent snapshots, mocked cloud merges/tombstones, and offline shell loading. No real customer tasks were changed and no real follow-up messages were sent.

Live Supabase connectivity and email delivery are not verified: automated checks isolate network requests and use sample data.
