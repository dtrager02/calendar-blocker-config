# calendar-blocker-config

This is the shared config for [calendar-blocker](https://github.com/dtrager02/calendar-block). Every hour, it finds time on everyone's Google Calendars for the events declared in [`config.yaml`](config.yaml) and books it.

**To add or change an event, open a pull request.** Once it's merged, the change takes effect on the next hourly run.

> This repository is **public**. Everything in it, including email addresses and event titles, descriptions and locations, can be read by anyone and stays in the git history forever.

## Adding an event
Add an entry under `requests:`:

```yaml
  - id: board-games            # unique and stable; renaming it deletes and recreates its blocks
    title: Board games
    description: Bring snacks. # optional
    location: Alex's place     # optional
    priority: 6                # higher wins when events compete for the same time
    attendees: [alex@gmail.com, sam@gmail.com]   # first person is the organizer and sends the invite
    recurrence: weekly         # daily | weekly | once (once needs `between: { from: YYYY-MM-DD, to: YYYY-MM-DD }`)
    duration: 3h               # 2h, 90m, 1h30m
    windows:
      - { days: [FRI, SAT], from: "18:00", to: "23:30" }
    strategies: { prefer-soonest: 1, minimal-gaps: 1 }   # optional; how to choose among possible slots
```

Every option is documented in the [app's README](https://github.com/dtrager02/calendar-block#configuration).

## Rules of thumb
- **Everyone in `attendees` must have onboarded first.** Ask the maintainer for an invite link. If anyone hasn't, the event is skipped.
- **Existing calendar events always win.** The app only books free time, and if nothing fits, it books nothing.
- **Priority.** A higher-priority event can take a slot from a lower-priority one when there's no other free slot, but never from an equal or higher one.
- **Edits update existing events.** Changing `title`, `description` or `location` updates every upcoming occurrence. Deleting an entry cancels all of its upcoming occurrences.
- **Recurring events are booked at most 30 days ahead.** One-time events are booked right away.

## Checks
Every pull request runs `validate`, which parses the config exactly as the server does. Fix any error it reports before asking for a merge.
