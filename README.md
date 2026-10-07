# tyler-actions

A personal command centre, not a task database. **Rule: Tyler should spend less time managing tasks than doing them.**

Single-file PWA (`index.html`). localStorage plus Supabase sync, using the same tables as v4.

## Navigation

Mobile: **Today · Inbox · (+) · People · Later**. Review, Done, Settings and theme sit under ⋯.
Desktop: the same items in the sidebar. Shortcuts: `N` capture · `F` focus · `/` search · `1–4` views · `Esc` close.

| Screen | The one decision it asks |
|---|---|
| Today | What should I do? |
| Inbox | Where does this belong? |
| People | Who needs chasing? |
| Later | Nothing (out of your head until it matters) |
| Focus | Do this one thing |

## Task lifecycle

```
Capture ─► Inbox (no date) ─► Mine (dated) ─► Done
                 │                 │  ▲
                 │                 ▼  │ "My move"
                 └──────────► Waiting on <person> (chase date)
Any open item ─► Parked (kept, never nags) · Dropped (kept in Done, guilt-free)
```

- **States:** `mine` · `waiting` · `parked` · `done` · `dropped`
- **One date field.** It is *soft* by default ("show on"). A soft item goes to Later until its date, then sits on Today. It never goes red.
  - *Hard* (`by Friday`, or the Hard deadline toggle) is a real deadline. It can be late.
  - For Waiting items, the date is the **chase date**.
- **Focus** (★): up to **3** items. Pinning a 4th asks you to swap one out.
- **Areas:** Sales · Leadership · Team · Shareholder · Personal. Optional, and inferred from keywords where possible.

## Today, in order

1. **Morning brief** (before 11:00, once a day): count, what's late, people to chase, a suggested first move, and **Start day**.
2. **Must move**: Focus items, plus hard deadlines due today or late. If nothing is pinned and the day is busy, the app suggests 2.
3. **Chase**: people with Waiting items whose chase date has arrived, grouped by person, with one **Chase all** button.
4. **Also today**: other items dated today or earlier. Six are shown, with "show more" for the rest.
5. **Footer**: Inbox count, coming up, and waiting-but-not-due.
6. **Wrap up** (after 16:00): every unfinished item gets one tap: Tomorrow · Next week · Waiting · Drop · Keep. Items moved 3× or older than 21 days are challenged ("still worth doing?").

## People

Each person card shows: to chase · waiting · to raise · last chased.

**Chase all** writes one short message covering everything outstanding, which you can send by Email (Outlook) or Share/Copy (WhatsApp/Teams). Every item is then logged as chased, and the next chase is set for 2 working days later.

People can be Team, Internal, Customer, Supplier or Other. Old "Other" contacts were merged in, so they now sync.

## Capture

Type, then Enter. Everything the parser infers is shown as a chip you can remove with ×. Multi-line paste creates multiple actions.

- `Chase Sheldon about Speedy quote Friday` → Waiting on Sheldon · "Speedy quote" · chase Fri · Sales
- `Review monthly sales report tomorrow` → Mine · tomorrow
- `by Thursday` = hard deadline · `!` = Focus · `#sales` `#lead` `#team` `#share` `#personal`

## Gestures

Each gesture has a button equivalent, so nothing depends on swiping.

- Swipe right = Done
- Swipe left = Tomorrow / Waiting / Later
- Long-press = Focus
- Tap = inline actions

Every change can be undone from the toast.

## Migration (v4 → v5, automatic, additive)

- The storage key is unchanged (`maxa_tasks_v4`). Old fields are kept, new ones are added (`state`, `hard`, `focus`, `waitingSince`, `chases`, `rolls`, `doneAt`, `v:5`).
- `done` → done. Assigned to someone → **waiting** on them. Everything else → mine.
- Contexts: Board→Shareholder, SLT→Leadership, Sales mgmt→Sales, Field support→Team.
- **Fresh start:** mine items dated more than 7 days ago move to Inbox (the original date is kept in `oldDue`), so Today opens calm.
- Priority is no longer shown. High still nudges the suggestions.
- A full **pre-redesign snapshot** (`backup_pre_v5`) is taken once and never rotated. You can restore it from Settings › Backups.

## Sync fixes

- v4 deleted every remote row that was missing locally, so a fresh or cleared device could wipe the cloud. Now remote deletes only follow tombstones.
- Only changed rows are pushed (debounced). Failed pushes are queued and retried.
- Dates use local time. v4 used UTC and showed yesterday between 00:00 and 01:00 BST.

## Removed

Removed: quotes, confetti, milestones, leaderboard and on-time %, drag-reorder, the filter/sort modal, the workspace switcher, a separate Focus and Hyper Focus Zone (now one Focus), and the separate Overdue/All tabs.
