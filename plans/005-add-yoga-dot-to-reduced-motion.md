# 005 — Cover the yoga progress dots in the reduced-motion block

- **Status**: DONE
- **Commit**: 78d086f
- **Severity**: LOW
- **Category**: Accessibility
- **Estimated scope**: 1 file (`www/index.html`), 1 CSS line added

## Problem

```css
/* www/index.html:248 — current */
.yogaDot { width: 8px; height: 8px; border-radius: 50%; background: rgba(255,255,255,0.25); transition: transform 0.15s, background 0.15s; }
```

This transition (a dot growing via `transform: scale(1.3)` and changing
`background-color` when it becomes the active/done pose indicator) is not
mentioned anywhere in the existing `prefers-reduced-motion` block:

```css
/* www/index.html:270-286 — current, full block for reference */
@media (prefers-reduced-motion: reduce) {
  main.viewEnter { animation: none; }
  .yogaGlyph { animation: none; }
  .yogaCount.countdown { animation: none; }
  .yogaPoseContent.yogaPoseEnter { animation: none; }
  .doneCard.yogaDoneEnter { animation: none; }
  .bParticle { animation: none; display: none; }
  #splashRing1, #splashRing2, #splashDot, #splashWord, #splashTagline { animation: none; opacity: 1; }
  .modalWrap, .modalSheet, #breathOverlay, #yogaOverlay, #ritualOverlay {
    transition-property: opacity;
    transition-duration: 0.15s;
  }
  .modalSheet, #breathOverlay, #yogaOverlay, #ritualOverlay { transform: none !important; }
  .card:active, .btn:active, .goalBtn:active, .chips button:active, nav button:active,
  #breathOverlay .btnRow button:active, #yogaOverlay .btnRow button:active,
  #breathOverlay .closeRow button:active, #yogaOverlay .closeRow button:active, #ritualOverlay .closeRow button:active {
    transform: none;
  }
}
```

Every other yoga animation in this file (`.yogaGlyph`, `.yogaCount.countdown`,
`.yogaPoseContent.yogaPoseEnter`, `.doneCard.yogaDoneEnter`, and the overlay's
own scale/fade) is already accounted for here. `.yogaDot` is a genuine
oversight, not a deliberate exclusion — it was added in an earlier pass before
this reduced-motion block existed and was never retrofitted into it.

## Target

```css
/* target — add inside the existing @media (prefers-reduced-motion: reduce) block */
.yogaDot { transition: none; }
```

## Repo conventions to follow

- This block's established pattern for small, non-essential motion is a flat
  `animation: none;` or `transition: none;` override (see `.yogaGlyph`,
  `.yogaCount.countdown` above) rather than swapping in an alternate
  "gentler" transition — follow that same flat-disable convention here rather
  than inventing a shorter/softer transition value.
- Per this project's own stated principle (documented in this file's
  `prefers-reduced-motion` block comments/commit history): reduced motion
  means fewer and gentler animations, not zero feedback. A disabled
  `.yogaDot` transition still leaves the `background-color`/`scale` END STATE
  fully intact (the dot still visibly shows which pose is current/done) — only
  the animated approach to that state is removed. This satisfies "keep
  transitions that aid comprehension, remove position/movement" because the
  dot's final state IS the comprehension aid; only the growth animation
  toward it is decorative.

## Steps

1. Open `www/index.html`, locate the `@media (prefers-reduced-motion: reduce)`
   block (search for `prefers-reduced-motion`).
2. Inside that block, immediately after the existing line
   `.doneCard.yogaDoneEnter { animation: none; }`, add a new line:
   ```css
   .yogaDot { transition: none; }
   ```

## Boundaries

- Do NOT change `.yogaDot`'s base (non-reduced-motion) rule at
  `www/index.html:248` — it stays exactly as-is; this plan only adds an
  override inside the media query.
- Do NOT remove or alter any other existing rule inside the
  `prefers-reduced-motion` block.
- If the block's current contents do not match the excerpt above (drift since
  commit `78d086f`), STOP and report instead of guessing where to insert the
  new line.

## Verification

- **Mechanical**: none (static file, no build step). Confirm the page loads
  with no console errors after the edit.
- **Feel check**: in the browser, open DevTools → Rendering panel → set
  "Emulate CSS media feature prefers-reduced-motion" to "reduce". Start a
  yoga routine and advance through a couple of poses. Confirm:
    - The progress dots still correctly show which pose is current (filled/
      larger) and which are done (dimmer) — the END STATE is unchanged.
    - The dot's scale/color change from one pose to the next now happens
      instantly (no 150ms grow/fade) instead of animating.
    - Everything else already covered by the reduced-motion block (glyph
      pulse, countdown pulse, pose fade, done-card pop, overlay open/close)
      remains disabled as before — this plan should not regress any of those.
  - Toggle the emulation back to "no preference" and confirm the dot's 150ms
    transition is back.
- **Done when**: with `prefers-reduced-motion: reduce` active, `.yogaDot`
  changes state with no animated transition, while its final visual state
  (which dot is on/done) is unaffected.
