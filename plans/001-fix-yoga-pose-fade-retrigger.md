# 001 — Fix yoga pose-transition fade not retriggering after the first pose

- **Status**: DONE
- **Commit**: 78d086f
- **Severity**: HIGH
- **Category**: Interruptibility
- **Estimated scope**: 1 file (`www/index.html`), 1 function (~6 line change)

## Problem

`www/index.html`'s `renderYogaStage(animate)` function drives the yoga session's
per-pose content. As of commit `78d086f`, `#yogaStage`'s active-pose markup is
split into two persistent child nodes that are created ONCE per session and then
reused across every pose change (this was a deliberate earlier fix so the
progress dots could keep their DOM identity):

```js
// www/index.html:1972-1991 — current
const pose = routine.poses[idx];
if (stageEl) {
  if (!stageEl.querySelector('.yogaDots')) {
    stageEl.innerHTML = `
      <div class="yogaPoseContent"></div>
      <div class="yogaDots">${routine.poses.map(() => `<span class="yogaDot"></span>`).join('')}</div>
    `;
  }
  const contentEl = stageEl.querySelector('.yogaPoseContent');
  contentEl.innerHTML = `
    <div class="yogaArtWrap">${poseArt(pose.key, 170)}</div>
    <div class="yogaPoseName">${escapeHtml(pose.name)}</div>
    <div class="yogaCue">${escapeHtml(pose.cue)}</div>
    <div class="yogaCount ${remaining <= 3 ? 'countdown' : ''}">${remaining}</div>
  `;
  contentEl.classList.toggle('yogaPoseEnter', !!animate);
  stageEl.querySelectorAll('.yogaDot').forEach((dot, i) => {
    dot.classList.toggle('on', i === idx);
    dot.classList.toggle('done', i < idx);
  });
}
```

`.yogaPoseEnter` triggers a CSS `@keyframes` entrance animation (see
`www/index.html:234-235`):

```css
@keyframes yogaPoseIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }
.yogaPoseContent.yogaPoseEnter { animation: yogaPoseIn 0.2s cubic-bezier(0.16,1,0.3,1) both; }
```

Because `contentEl` (`.yogaPoseContent`) is now a **persistent node** reused
across the whole session, `contentEl.classList.toggle('yogaPoseEnter', !!animate)`
only has an effect the first time it runs. `Element.classList.toggle(token, true)`
is equivalent to `classList.add(token)` — if the class is already present, it is
a no-op: no attribute mutation occurs, so the browser never re-triggers the CSS
`animation`. CSS keyframe animations only (re)play on a fresh application of the
`animation` property via a DOM/style mutation, not merely "the class is present."

Sequence in practice:
1. `startYogaSession()` (`www/index.html:1943-1948`) calls `renderYogaStage(true)`. `.yogaPoseEnter` is absent → added → fade plays. Correct.
2. `advanceYogaPose()` (`www/index.html:2009-2016`) calls `renderYogaStage(true)` for pose 2. `.yogaPoseEnter` is *already present* from step 1 → `toggle(cls, true)` is a no-op → **no animation plays**, despite `animate` being `true` and the CSS being otherwise correct.
3. Every subsequent pose (3, 4, 5) has the same problem.

Net effect: only the very first pose of a session ever visibly fades in. This is
the headline feature of a prior improvement pass and it is currently non-functional
for 4 out of 5 poses in every routine.

## Target

Force a style recalculation between removing and re-adding the class, exactly
like the app's own `replayViewEnter()` helper already does for the main-content
fade (`www/index.html`, function `replayViewEnter`):

```js
// exemplar already in this file — replayViewEnter()
function replayViewEnter() {
  const main = document.getElementById('main');
  main.classList.remove('viewEnter');
  void main.offsetWidth;
  main.classList.add('viewEnter');
}
```

Target code for the yoga pose block:

```js
// target — www/index.html, inside renderYogaStage(), replacing the single toggle line
if (animate) {
  contentEl.classList.remove('yogaPoseEnter');
  void contentEl.offsetWidth;
  contentEl.classList.add('yogaPoseEnter');
} else {
  contentEl.classList.remove('yogaPoseEnter');
}
```

No CSS changes are required — `@keyframes yogaPoseIn` and
`.yogaPoseContent.yogaPoseEnter` at `www/index.html:234-235` are correct as-is.

## Repo conventions to follow

- This exact remove → forced reflow (`void el.offsetWidth`) → re-add pattern is
  already established in this file for retriggering a CSS keyframe animation on
  a persistent element. See `replayViewEnter()` (search for that function name)
  and the double-`requestAnimationFrame` variant used in `openOverlay(id)` for
  the analogous "go from not-applied to applied" case.
- Do not introduce a new easing/duration — `yogaPoseIn`'s existing
  `0.2s cubic-bezier(0.16,1,0.3,1)` is correct and matches the app's established
  entrance curve; this plan only fixes the JS retrigger, not the animation itself.

## Steps

1. Open `www/index.html`, locate `renderYogaStage(animate)` (search for
   `function renderYogaStage`).
2. Find the line:
   ```js
   contentEl.classList.toggle('yogaPoseEnter', !!animate);
   ```
3. Replace it with:
   ```js
   if (animate) {
     contentEl.classList.remove('yogaPoseEnter');
     void contentEl.offsetWidth;
     contentEl.classList.add('yogaPoseEnter');
   } else {
     contentEl.classList.remove('yogaPoseEnter');
   }
   ```
4. Do not change anything else in `renderYogaStage()` — the dots-update block
   immediately below (`stageEl.querySelectorAll('.yogaDot').forEach(...)`) is
   unrelated and already correct (it uses CSS `transition`, not `animation`,
   which does not have this retrigger problem).

## Boundaries

- Do NOT touch the `.yogaDots`/`.yogaDot` logic — it is already correct (verified
  in the audit; CSS transitions retarget from current value on every class
  change, unlike keyframe animations).
- Do NOT touch the `intro` or `done` branches of `renderYogaStage()` — this bug
  is specific to the active-pose branch's persistent `.yogaPoseContent` node.
- Do NOT change the `yogaPoseIn` keyframe, its duration, or its easing curve.
- Do NOT add this reflow-retrigger pattern anywhere else in the file — scope is
  this one call site only.
- If the code at `www/index.html` around `renderYogaStage()` does not match the
  excerpts above (drift since commit `78d086f`), STOP and report the mismatch
  instead of improvising.

## Verification

- **Mechanical**: none (no build/typecheck step in this project — it is a
  single static HTML file). Confirm the file still opens without a JS syntax
  error: `node --check` is not applicable to HTML; instead load the page and
  confirm no console errors on load.
- **Feel check**: open the app locally (`www/serve.ps1` or equivalent static
  server), start a yoga routine (Utforsk → Yoga → any routine → Start økten),
  then either wait for a pose to complete naturally or tap "Neste" repeatedly.
  Confirm:
    - Every pose transition (not just the first) shows the glyph/name/cue
      fading up from `translateY(6px)`/`opacity: 0`, not just pose 1→2.
    - Rapidly tapping "Neste" several times in a row does not throw a console
      error and each tap still shows a (possibly very short, since retriggered
      mid-flight) fade — it should never look like a hard instant swap.
    - Pausing and resuming (`Pause`/`Fortsett` button) does NOT retrigger the
      fade — the pose content should stay static, only the button label
      changes.
  - In DevTools (or the browser preview tool's `javascript_tool`), you can also
    verify mechanically:
    ```js
    // after starting a session and calling advanceYogaPose() twice in a row
    const el = document.querySelector('.yogaPoseContent');
    // classList should contain 'yogaPoseEnter' after each animate=true render,
    // and getAnimations() (if supported) should show a fresh, running yogaPoseIn
    // animation each time — not a completed/finished one.
    el.getAnimations().map(a => a.playState);
    ```
- **Done when**: the fade visibly plays on every pose transition within a
  5-pose routine, not just the first, confirmed by watching (or slow-motion
  screen recording) a full routine start to finish.
