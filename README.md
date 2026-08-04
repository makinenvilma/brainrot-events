# Fortnite Brainrot Events

Live countdowns to the repeating events in Fortnite Brainrot. Five timers, each on
its own interval, with a progress bar showing how far into the current cycle each one
is.

The whole thing is one HTML file: no dependencies, no build step, no server.

Live at https://brainrot-events.vercel.app/

## Status

Finished and in use. Timers are edited by opening the file.

Working right now:

- Five events counting down at once, redrawn four times a second
- Each event has its own interval and its own starting point
- Next occurrence shown as a timestamp in the viewer's own timezone
- Progress bar showing how far into the current cycle you are
- Anchors can be written in UTC or in the viewer's local time

Not done yet:

- Events are edited in the `EVENTS` array inside `index.html`. There is no interface
  for it and nothing is saved anywhere.
- No sound, no notification, no countdown in the tab title. You have to be looking at
  the page.
- Nothing checks the intervals against the actual game. If an event changes, the page
  keeps counting down to the wrong moment.
- The countdown is arithmetic from one fixed instant, so daylight saving moves what
  the timers land on in wall clock terms. There is a section on this below.

## Tech

Plain HTML, CSS and JavaScript in a single file. Dark theme, CSS grid that reflows to
one column on a phone. Timing is `Date.now()` and modulo arithmetic against each
event's anchor. Display goes through `toLocaleString()`, so everyone sees their own
timezone no matter how the anchor was written.

## Running it

Open `index.html` in a browser.

To put it online, any static host will do. The live version is on Vercel, which needs
no configuration for a repo like this.

## Configuring the timers

Everything lives in the CONFIGURATION block near the top of the script:

```js
const EVENTS = [
  {
    name: "Underwater Event",
    intervalMs: h(3),
    anchor: { mode: "UTC", year: 2025, month: 10, day: 3, hour: 7, minute: 0, second: 0 }
  },
```

Intervals use helper functions, so you write the unit instead of counting
milliseconds:

```js
s(30)   // 30 seconds
m(45)   // 45 minutes
h(3)    // 3 hours
d(2)    // 2 days
w(1)    // 1 week
```

The anchor is any one moment the event is known to have happened. Everything else is
counted from there, forwards and backwards, so it is fine for it to be in the past.
Months are 1 to 12 and hours are 0 to 23.

`mode: "UTC"` reads the anchor as a UTC timestamp, which is what you want when the
event fires at the same instant for everybody. That is the case for all five events
here. `mode: "LOCAL"` reads it in whatever timezone the viewer is in, which is only
useful for something that follows each person's own clock.

## A note on daylight saving

Once the anchor is fixed, the countdown is pure arithmetic: anchor plus some number of
whole intervals. Nothing is recalculated against a calendar. So an event on a 24 hour
interval that lands at 09:00 today will land at 08:00 or 10:00 after the clocks
change, in LOCAL mode just as much as in UTC mode. LOCAL only changes how the anchor
itself is read, not how the repeats are counted from it.

Keeping an event at a fixed local wall clock time would mean recalculating each
occurrence against the calendar rather than multiplying an interval. This does not do
that, which does not matter for events that fire on a global timer.

## Layout of the code

```
index.html    the entire thing: styles, config, timer logic, markup
```

Inside the script, in order: the interval helpers, the `EVENTS` array, duration
formatting, `nextOccurrence` and `progressBetween` for the arithmetic, card creation,
and a `tick` on a 250 ms interval that redraws everything.

## Next up

1. Editing events on the page instead of in the file
2. Saving them, so edits survive a refresh
3. A notification, or at least the countdown in the tab title
4. Fix the "weeksly" typo in the config comments
