# Handoff: Workout Tracker (mobile)

## Overview
A single-user mobile workout logger. The user works through a planned workout set by set: each set shows the **target reps** large, the user enters the **weight** they lifted, taps **Log set**, and the app shows a **rest timer** before the next set. A summary appears when every set is logged.

## About the design files
`prototype/Workout Tracker.html` is a **design reference built in HTML**: a working prototype that shows the intended look and behavior. It is not production code. Rebuild it in the target stack. If there's no existing codebase, a good default is **React + Vite + TypeScript as an installable PWA** (it's a phone-first app, and the user currently runs it from the home screen). React Native or Expo also works if a native app is wanted.

`prototype/index.html` is the same prototype bundled into one offline file. Open it in a browser to click through the flow.

## Fidelity
**High fidelity.** Colors, type, spacing and interactions are final. Match them closely.

---

## Screens

The layout fills the screen on devices ≤500px wide. On desktop the prototype draws a 390×844 phone frame, which is only for presentation, so don't build the frame. Horizontal page padding is **22px** on every screen. Background `#0F0F0E`, text `#F3F1EA`.

Top safe area: `calc(env(safe-area-inset-top) + 12px)` in the browser, and `max(env(safe-area-inset-top), 47px)` when running standalone (from the home screen). Bottom: `max(env(safe-area-inset-bottom), 16px)`.

### 1. Overview (workout list)
- **Purpose:** see today's plan, start or resume, or jump to any exercise.
- **Layout:** a column with eyebrow → title → exercise list (flex:1) → primary button.
- Eyebrow `TUESDAY · UPPER A`: Plex Mono 500 11px, letter-spacing .12em, uppercase, `#8D8A82`, margin-top 18px.
- Title `PUSH & PULL` (2 lines): Archivo 800 54px/0.92, font-stretch 68%, uppercase, letter-spacing −.01em, margin 10px 0 26px.
- Exercise row (button): grid `28px 1fr auto`, gap 12px, padding 16px 0, bottom border 1px `#2A2A27`. Pressed state: opacity .6.
  - Index `01`: Plex Mono 500 12px `#8D8A82`.
  - Name: Archivo 700 19px.
  - Meta `4 × 8 · last 60 kg`: Plex Mono 400 12px `#8D8A82`, 4px below the name.
  - Progress `2/4`, or `Done` in accent: Plex Mono 600 12px, `#8D8A82` until complete, then `#FF5A1F`.
  - Tapping a row opens that exercise at its first unlogged set.
- Primary button: height 64px, radius 18px, bg `#FF5A1F`, text `#0F0F0E`, Archivo 800 20px, stretch 80%, uppercase, letter-spacing .04em, margin-top 16px. Label `Start workout`, or `Resume workout` once any set is logged. Pressed state: scale .98.

### 2. Log set
- **Purpose:** see the target reps, enter the weight, log the set.
- **Layout:** a column that scrolls if the screen is too short. From top to bottom:
  1. **Top bar** (44px): `‹ Workout` (Archivo 600 15px `#8D8A82`, returns to Overview) on the left, `Exercise 1/4` (Plex Mono 500 12px `#8D8A82`) on the right.
  2. **Exercise name:** Archivo 800 34px/1, stretch 72%, uppercase, margin-top 6px.
  3. **Set chips:** grid of 4 columns `minmax(0,1fr)`, gap 6px, margin-top 14px. Chip height 40px, radius 10px, 1px border, Plex Mono 600 11px.
     - Current: bg/border `#F3F1EA`, text `#0F0F0E`, label `Set N`.
     - Logged: bg/border `#1B1B19`, text `#FF5A1F`, label `62.5×8`.
     - Upcoming: transparent bg, border `#2A2A27`, text `#55534E`, label `Set N`.
  4. **Reps hero row** (margin-top 8px, space-between): a − button, the rep number, a + button.
     - The ± buttons are 48px circles with a 1px `#2A2A27` border, Archivo 500 24px `#8D8A82`, and pressed bg `#1B1B19`. Reps can't go below 1.
     - Number: Archivo 900 **148px**/1, stretch 62%, letter-spacing −.03em, followed by `REPS` (Archivo 700 18px, stretch 80%, uppercase, `#8D8A82`, gap 8px, baseline-aligned).
  5. **Weight card:** bg `#1B1B19`, radius 18px, padding 10px, gap 10px, margin-top 4px.
     - Side buttons: 52×52, radius 12px, bg `#262624`, pressed `#33332F`, Plex Mono 600 14px, labels `−2.5` / `+2.5` (`−5` / `+5` in lb).
     - Center: the value in Archivo 800 40px, stretch 75%, followed by the unit (Archivo 600 14px `#8D8A82`). Below it, `last time 60 kg × 8` in Plex Mono 400 11px `#8D8A82`.
     - The value is `#F3F1EA` when prefilled. It turns accent `#FF5A1F` while the user is typing.
  6. **Keypad:** 3 columns, gap 6px, margin-top 10px. Keys `1–9 . 0 ⌫`, height **50px**, radius 12px, Archivo 600 22px, transparent bg, pressed `#1B1B19`.
  7. A flex spacer, then the **Log button**: same style as the primary button, margin-top 12px. Label `Log set {N} · {weight} {unit}`.
- **Short screens** (mobile and height ≤760px): reps 96px, keys 44px.

### 3. Rest (overlay covering the whole screen)
- **Purpose:** count down the rest, then start the next set.
- Background `#FF5A1F` while counting. It switches to `#F3F1EA` at 0, with a .4s background transition. All text is `#0F0F0E`, and the status bar and home indicator switch to dark.
- **Top row** (44px): `✓ SET 1 · 62.5 KG × 8` (Plex Mono 600 12px, letter-spacing .1em, uppercase) on the left, an underlined `Undo` on the right (Plex Mono 600 13px). Undo removes that log and returns to Log set.
- **Ring:** centered in flex:1. SVG 320×320 (240 on short screens), rotated −90°. Circle r=150, stroke 10px. Track `rgba(15,15,14,.14)`, progress `#0F0F0E` with a round cap. dashoffset = C·(1 − left/total), with a 1s linear transition.
  - Inside the ring: label `REST` or `TIME’S UP` (Archivo 700 16px, stretch 80%, uppercase, letter-spacing .2em), then the time `1:30` or `GO` (Archivo 900 112px/1, stretch 62%, tabular numbers; 84px on short screens).
- **Up next block:** 2px top border `#0F0F0E`, padding 16px 0, margin-bottom 14px.
  - `UP NEXT` in Plex Mono 500 11px, letter-spacing .12em.
  - Exercise name in Archivo 800 24px, stretch 75%, uppercase.
  - `Set 2 of 4 · 8 reps @ 62.5 kg` in Plex Mono 500 13px.
- **Actions while counting:** a grid `1fr 1fr 1.4fr`, gap 8px, height 60px, radius 16px.
  - `−15s` and `+15s`: 2px `#0F0F0E` border, Plex Mono 700 16px.
  - `SKIP REST`: bg `#0F0F0E`, text `#F3F1EA`, Archivo 800 17px.
- **At 0:** a single full-width button `START SET N` (64px, radius 18px, bg `#0F0F0E`, text `#F3F1EA`).

### 4. Done (summary)
- Eyebrow `WORKOUT COMPLETE` in accent `#FF5A1F`, styled like the Overview eyebrow. Title `NICE / WORK.` in the same style as the Overview title.
- **Stats row:** 3 columns between 1px `#2A2A27` top and bottom borders, padding 16px 0.
  - Values in Archivo 800 30px, stretch 70%: `14`, `6.2k`, `48m`.
  - Labels in Plex Mono 400 11px `#8D8A82`: `sets`, `kg volume`, `duration`.
- **Per-exercise rows:** name (Archivo 700 16px) on the left, `60×8  62.5×8 …` (Plex Mono 400 12px `#8D8A82`) on the right, padding 13px 0, bottom border.
- `DONE` button: 64px, radius 18px, 1px `#3A3A36` border, transparent, Archivo 800 18px. **This currently wipes the session.** Replace that with saving to history (see Next steps).

### Status bar (in the app)
On the right: elapsed workout time `mm:ss` in Plex Mono 500 12px, shown from the first set until Done.

---

## Interactions & behavior
- **Start/Resume:** goes to the first unlogged set (starting from set 1 of exercise 1).
- **Weight prefill:** the previous set's logged weight for this exercise, otherwise the exercise's "last time" weight.
- **Keypad:** the first key press after a prefill or ± replaces the value; later presses append. Max 4 digits and one decimal point. `⌫` deletes one character, and deleting everything leaves `0`.
- **±:** ±2.5 kg or ±5 lb, never below 0, rounded to 0.1. Using ± resets to the "prefilled" state, so the next key press replaces the value.
- **Log set:** saves `{weight, reps}` for the set. If other sets remain, it goes to Rest with duration = `restSeconds` override, or the exercise's `rest`. If it was the last unlogged set, it goes to Done.
- **Next set:** the next unlogged set after the current one in workout order, wrapping to earlier skipped sets.
- **Rest timer:** store `restEnd` (an epoch ms timestamp) and calculate `left = ceil((restEnd − now)/1000)`, so the countdown stays correct while the app is in the background. −15s and +15s shift `restEnd`, and +15s may grow `restTotal` so the ring stays valid.
  - Suggested additions for production: a vibration or sound plus a local notification at 0.
- **Pressed states:** `scale(.98)` on primary buttons. Lists and links use opacity .5–.6. Small buttons get the darker background noted above.
- **Units:** kg or lb. The lb version of the plan's "last" weights is `round(kg·2.2046/5)·5`.

## State
```ts
type SetLog = { w: number; r: number };
state = {
  screen: 'overview' | 'set' | 'rest' | 'done',
  ex: number, set: number,               // current position
  weight: string, fresh: boolean,       // input buffer; fresh = next key replaces
  reps: number,
  logs: Record<`${ex}-${set}`, SetLog>,
  restLeft: number, restTotal: number, restEnd: number | null,
  next: { ex: number; set: number } | null,
  startedAt: number | null,             // workout start, ms
}
```
**Persistence (current):** the whole state above is saved to `localStorage['workout-tracker-v1']` on every change and restored on load, so a reload resumes mid-workout. There's no history across workouts yet.

**Settings currently exposed as design tweaks** (turn these into a settings screen):
- `units` (`kg` or `lb`)
- `inputMode` (`keypad` or `stepper`; stepper hides the keypad)
- `restSeconds` (0 = use each exercise's own rest time)

## Sample plan data (placeholder)
```js
[
  { name: 'Bench Press',       sets: 4, reps: 8,  last: 60,   rest: 90  },
  { name: 'Barbell Row',       sets: 4, reps: 10, last: 50,   rest: 90  },
  { name: 'Overhead Press',    sets: 3, reps: 8,  last: 37.5, rest: 75  },
  { name: 'Romanian Deadlift', sets: 3, reps: 10, last: 80,   rest: 120 },
]
```

## Design tokens
**Colors**
- `bg` #0F0F0E
- `surface` #1B1B19
- `surface-2` #262624
- `surface-pressed` #33332F
- `line` #2A2A27
- `line-strong` #3A3A36
- `text` #F3F1EA
- `muted` #8D8A82
- `disabled` #55534E
- `accent` #FF5A1F
- `on-accent` #0F0F0E

**Type.** Two fonts are used, both from Google Fonts:
- **Archivo**, a variable font. Use the `wdth` axis (62–125) through `font-stretch`, with weights 400–900.
- **IBM Plex Mono**, weights 400, 500 and 600.

Archivo condensed (stretch 62–80%) is for display text and numbers. Plex Mono is for labels and metadata.

**Radii:** 10 (chips), 12 (keys, small buttons), 16 (rest buttons), 18 (primary buttons, cards), 50% (round buttons).

**Heights:** buttons 64px primary, 60px secondary; keys 50/44px; set chips 40px.

**Spacing:** page padding 22px; gaps of 6, 8, 10, 12 and 16px.

## Assets
None. No images or icons; the glyphs are text characters (‹ − + ⌫ ✓).

## Files
- `prototype/Workout Tracker.html`: the readable source of the prototype. The markup has `{{ }}` placeholders that the logic script at the bottom fills in, and that script (a JS class) holds all the behavior.
- `prototype/index.html`: a self-contained build you can open in any browser.

## Suggested next steps
1. **Workout history:** on Done, add a `{date, durationSec, sets[]}` record to the history instead of wiping the session. Add a History screen, and fill "last time" from the most recent history entry for each exercise.
2. **Plan editor:** add, remove and reorder exercises and edit sets, reps and rest.
3. **Backup:** export/import as JSON. Optionally add cloud sync (for example Supabase with email sign-in) for use on more than one device.
4. **PWA:** a manifest, icons and a service worker, so the app works offline and installs to the home screen. Self-host the fonts for offline use.
5. **Rest alerts:** vibration and a notification when the timer reaches 0.
