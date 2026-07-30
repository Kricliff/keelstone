# 002 — Pause the glyph pulse and countdown pulse when the yoga session is paused

- **Status**: DONE
- **Commit**: 78d086f
- **Severity**: MEDIUM
- **Category**: Purpose & frequency (state fidelity)
- **Estimated scope**: 1 file (`www/index.html`), 2 CSS rules added, 1 JS function edited

## Problem

Two yoga animations are pure CSS infinite loops with no connection to the
session's `paused` state:

```css
/* www/index.html:239-240 — current */
.yogaGlyph { animation: yogaGlyphPulse 3.2s ease-in-out infinite; transform-origin: 50% 50%; filter: drop-shadow(0 0 14px rgba(217,178,92,0.35)); }
@keyframes yogaGlyphPulse { 0%, 100% { transform: scale(0.92); opacity: 0.8; } 50% { transform: scale(1.03); opacity: 1; } }
```

```css
/* www/index.html:244-245 — current */
.yogaCount.countdown { color: var(--sand); animation: yogaCountPulse 1s ease-in-out infinite; }
@keyframes yogaCountPulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.15); } }
```

`toggleYogaPause()` only flips a JS flag and re-renders (`www/index.html`,
function `toggleYogaPause`):

```js
// www/index.html:2017 — current
function toggleYogaPause() { if (!yogaState) return; yogaState.paused = !yogaState.paused; renderYogaStage(); }
```

`startYogaTimer()` correctly stops incrementing the countdown while paused
(`www/index.html:1997-2008`, the `if (!yogaState || yogaState.paused || ...) return;`
guard at the top of the interval callback), so the *number* correctly freezes.
But the glyph keeps breathing and, if the pose was in its final 3 seconds when
paused, the countdown digit keeps pulsing gold — both visually say "still
running" while the session is actually frozen. A user glancing at the screen
mid-pause has no reliable visual cue that anything stopped besides a static
number, which is easy to miss.

## Target

Gate both loop animations with `animation-play-state`, controlled by a
`.paused` class on `#yogaStage`, set every time `renderYogaStage()` runs:

```css
/* target — add near the existing .yogaGlyph/.yogaCount.countdown rules */
#yogaStage.paused .yogaGlyph,
#yogaStage.paused .yogaCount.countdown {
  animation-play-state: paused;
}
```

```js
// target — inside renderYogaStage(), active-pose branch, after the existing
// stageEl null-check block that builds/updates content and dots
stageEl.classList.toggle('paused', !!paused);
```

(`paused` is already destructured from `yogaState` at the top of
`renderYogaStage()` — no new variable needed.)

## Repo conventions to follow

- This file already gates other per-state CSS purely through class toggles set
  inside `renderYogaStage()` (e.g. `.yogaDot`'s `.on`/`.done` classes,
  `.yogaCount`'s `.countdown` class) — follow the same pattern: toggle a class,
  let CSS react to it, no inline styles.
- Do not add a JS-driven pause/resume of the animation (e.g. reading
  `getAnimations()` and calling `.pause()`/`.play()` in JS) — `animation-play-state`
  in CSS is simpler, is not layout/perf-affecting, and matches this app's
  CSS-first approach to state-driven visuals (see `prefers-reduced-motion`
  handling and every existing `.doneCard`/`.yogaDot` state class for the same
  philosophy: JS toggles a class, CSS owns the visual consequence).

## Steps

1. Open `www/index.html`, locate the `.yogaGlyph`/`@keyframes yogaGlyphPulse`
   and `.yogaCount.countdown`/`@keyframes yogaCountPulse` rules (search for
   `yogaGlyphPulse`).
2. Immediately after the `@keyframes yogaCountPulse { ... }` rule, add:
   ```css
   #yogaStage.paused .yogaGlyph,
   #yogaStage.paused .yogaCount.countdown {
     animation-play-state: paused;
   }
   ```
3. Open the `renderYogaStage(animate)` function (search for
   `function renderYogaStage`).
4. Inside the active-pose branch (the `if (stageEl) { ... }` block that builds
   `.yogaPoseContent` and the dots — this is the same block Plan 001 touches;
   if Plan 001 has already been applied, add this after that plan's change,
   otherwise add it after the existing `contentEl.classList.toggle('yogaPoseEnter', ...)`
   line), add:
   ```js
   stageEl.classList.toggle('paused', !!paused);
   ```
5. Also apply the same toggle in the `intro` branch (so pausing is impossible
   there is fine, but a stale `.paused` class must not leak in from a previous
   session) — actually the `intro` branch fully replaces `stageEl.innerHTML`,
   which does NOT clear a class on `stageEl` itself (only its children). Add
   `stageEl.classList.remove('paused');` as the first line inside the `if (intro)`
   block, before its `stageEl.innerHTML = ...` assignment, to guarantee no
   stale `.paused` state survives into a fresh session.

## Boundaries

- Do NOT pause `.yogaDot`'s CSS transition — dots use `transition`, not
  `animation`, and are not a continuous loop; they are unaffected by this plan
  and Plan 001/005 own that element.
- Do NOT change the 3.2s / 1s durations or the `ease-in-out` easing of either
  keyframe — only gate playback via `animation-play-state`.
- Do NOT add a JS interval or rAF loop to drive this — CSS `animation-play-state`
  is sufficient and is the target end state.
- If `renderYogaStage()`'s structure has changed since commit `78d086f` (e.g.
  the `if (stageEl) { ... }` active-pose block no longer looks like the excerpt
  in Plan 001), STOP and report instead of guessing where to insert the toggle.

## Verification

- **Mechanical**: none (static HTML/CSS/JS, no build step). Load the page,
  confirm no console errors.
- **Feel check**: start a yoga routine, let it sit on a pose for a couple of
  seconds so `.yogaGlyph`'s pulse is visibly mid-cycle, then tap "Pause".
  Confirm:
    - The glyph's gentle scale/opacity pulse visibly freezes in place (does
      not jump to a rest state, does not keep breathing).
    - If paused during the last 3 seconds of a pose (countdown showing gold),
      the countdown number's scale-pulse also freezes.
    - Tapping "Fortsett" (resume) makes both animations continue smoothly from
      wherever they were paused, not restart from 0% — this is the default
      behavior of `animation-play-state: paused` → `running` and should require
      no extra code, but confirm it visually since a play-state resume can look
      like a restart if implemented incorrectly.
  - In DevTools Animations panel (or `document.querySelectorAll('.yogaGlyph, .yogaCount.countdown')[0].getAnimations()[0].playState`), confirm the state reads `"paused"` while paused and `"running"` after resuming.
- **Done when**: both loops visibly stop moving while the session is paused
  and resume smoothly (not restart) when unpaused, for both the glyph and a
  countdown-pulse caught mid-cycle.
