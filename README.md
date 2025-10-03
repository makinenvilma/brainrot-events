# Fortnite Brainrot Events — Static Timers

A single‑file **HTML + JS** page that shows repeating Fortnite “brainrot” events as live countdowns. No build tools, no dependencies — just open `index.html`.

> Demo: [https://brainrot-events.vercel.app/](https://brainrot-events.vercel.app/)

## Features

* **Zero‑dependency** static page — works by double‑clicking `index.html`.
* **Per‑event anchors**: each timer can start from its own date & time.
* **UTC or LOCAL** mode per event.
* Clean UI: countdown, next occurrence timestamp, progress bar.

## Files

* `index.html` — the app (all logic inline).

## Installation & Setup

### Local

1. Download `index.html`.
2. Open it in your browser (double‑click).

## Configure timers

Open `index.html` and edit the **CONFIGURATION** block. You already have **per‑event anchors**:

```js
const EVENTS = [
  {
    name: "Underwater Event",
    intervalMs: h(3),
    anchor: { mode: "UTC", year: 2025, month: 10, day: 3, hour: 6, minute: 0, second: 0 }
  },
```

### Intervals shorthand

Use the helper functions to set repeat intervals:

```js
const s = (n) => n * 1000;       // seconds
const m = (n) => n * 60 * 1000;   // minutes
const h = (n) => n * 60 * 60 * 1000; // hours
const d = (n) => n * 24 * 60 * 60 * 1000; // days
const w = (n) => n * 7 * 24 * 60 * 60 * 1000; // weeks
```

### Anchor object

* `mode`: `"UTC"` or `"LOCAL"`.
* `year`, `month` (1–12), `day`, `hour` (0–23), `minute`, `second`.
* **Display** is always localized (`toLocaleString()`), but math uses your anchor.

**UTC vs LOCAL**

* `UTC` → exact same instant globally. Local time on page will shift with DST.
* `LOCAL` → interpreted in the viewer’s timezone. Good for “always 09:00 local”.

**Example**: *Start at 09:00 Helsinki time on Oct 3, 2025.*

* Helsinki is UTC+3 on 2025‑10‑03 → 09:00 local = 06:00 UTC.
* Use either:

```js
anchor: { mode: "LOCAL", year: 2025, month: 10, day: 3, hour: 9, minute: 0, second: 0 }
// or
anchor: { mode: "UTC",   year: 2025, month: 10, day: 3, hour: 6, minute: 0, second: 0 }
```

## Timezone & DST tips

* Months are **1–12** in config; code converts internally.
* If you want *fixed local wall‑clock time* (e.g., always 09:00 Helsinki), use `mode: "LOCAL"`.
* If you want *one global moment* (e.g., weekly server reset), use `mode: "UTC"`.
* Around DST changes, `UTC` anchors will display one hour earlier/later locally; `LOCAL` anchors won’t.

---

**Made for quick Fortnite event timing — stay locked in, the grind never stops.**
