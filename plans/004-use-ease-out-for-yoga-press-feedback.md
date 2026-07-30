# 004 — Use ease-out (not bare ease) for yoga overlay press-feedback transitions

- **Status**: DONE
- **Commit**: 78d086f
- **Severity**: LOW
- **Category**: Easing & duration
- **Estimated scope**: 1 file (`www/index.html`), 2 CSS value changes

## Problem

```css
/* www/index.html:229 — current */
#yogaOverlay .closeRow button { background: rgba(255,255,255,0.15); color: #fff; border: none; border-radius: 999px; width: 36px; height: 36px; font-size: 1rem; cursor: pointer; flex-shrink: 0; transition: transform 0.1s ease; }
```

```css
/* www/index.html:252 — current */
#yogaOverlay .btnRow button { flex: 1; padding: 12px; border-radius: 12px; font-family: inherit; font-size: 0.9rem; font-weight: 600; cursor: pointer; border: none; transition: transform 0.1s ease; }
```

Both use bare `ease` for a press-feedback `transform` transition. This
codebase's own audit standard specifies press feedback as
`transform: scale(0.97)` on `:active` with `transition: transform 160ms ease-out`
— i.e. the reference implementation uses `ease-out`, not `ease`, for exactly
this kind of entering-a-pressed-state transition. `ease` (the CSS default,
roughly `cubic-bezier(0.25, 0.1, 0.25, 1)`) has a softer start than `ease-out`
and is the documented choice for hover/color changes, not transform-based
press feedback.

This is a very low-perceptibility difference at 100ms on a small button — it
will not look "wrong" to a user — but it is a documented deviation from this
project's own stated convention and is trivial to correct while other yoga
plans are already touching these exact two lines.

## Target

```css
/* target — www/index.html:229 */
#yogaOverlay .closeRow button { background: rgba(255,255,255,0.15); color: #fff; border: none; border-radius: 999px; width: 36px; height: 36px; font-size: 1rem; cursor: pointer; flex-shrink: 0; transition: transform 0.1s ease-out; }
```

```css
/* target — www/index.html:252 */
#yogaOverlay .btnRow button { flex: 1; padding: 12px; border-radius: 12px; font-family: inherit; font-size: 0.9rem; font-weight: 600; cursor: pointer; border: none; transition: transform 0.1s ease-out; }
```

## Repo conventions to follow

- Press-feedback transitions elsewhere in this file already use bare `ease`
  too (e.g. `.card`, `.btn`, `nav button`, `.chips button`, `.goalBtn`) — this
  plan does NOT attempt to fix those; it is scoped to the two yoga-specific
  rules only, per the audit's yoga-only scope. Treat the app-wide
  `ease` → `ease-out` consolidation as a separate future plan if the user
  wants it broadened later — do not expand scope here.
- Keep the `0.1s` duration unchanged — it is within the 100–160ms press
  feedback budget already.

## Steps

1. Open `www/index.html`, locate `#yogaOverlay .closeRow button` (search for
   that exact selector).
2. Change `transition: transform 0.1s ease;` to
   `transition: transform 0.1s ease-out;` on that rule only.
3. Locate `#yogaOverlay .btnRow button` (search for that exact selector).
4. Change `transition: transform 0.1s ease;` to
   `transition: transform 0.1s ease-out;` on that rule only.

## Boundaries

- Do NOT change any other `ease` occurrence in the file — this plan is
  scoped to exactly these two yoga-overlay button rules.
- Do NOT change the `0.1s` duration or the `scale()` values on `:active` (see
  Plan 003 for the closeRow scale value fix — apply both plans if both are
  selected; they touch adjacent but distinct parts of the same two rules and
  do not conflict).
- If either selector's current value does not match the excerpt above (drift
  since commit `78d086f`), STOP and report instead of guessing.

## Verification

- **Mechanical**: none (static file, no build step). Confirm the page loads
  with no console errors after the edit.
- **Feel check**: this is a genuinely subtle change at 100ms duration — it is
  not expected to be visually distinguishable in normal use. Verification here
  is mechanical/code-level, not a feel check:
  - Confirm both rules read `ease-out` via `getComputedStyle`:
    ```js
    getComputedStyle(document.querySelector('#yogaOverlay .closeRow button')).transitionTimingFunction
    getComputedStyle(document.querySelector('#yogaOverlay .btnRow button')).transitionTimingFunction
    ```
    Both should report `"cubic-bezier(0, 0, 0.2, 1)"` (the browser's resolved
    form of the `ease-out` keyword), not `"ease"` (`"cubic-bezier(0.25, 0.1, 0.25, 1)"`).
- **Done when**: both computed `transitionTimingFunction` values resolve to
  `ease-out`'s cubic-bezier, not `ease`'s.
