# 003 — Bring the yoga overlay's close-button press scale within the subtle range

- **Status**: DONE
- **Commit**: 78d086f
- **Severity**: MEDIUM
- **Category**: Physicality & origin
- **Estimated scope**: 1 file (`www/index.html`), 1 CSS value change

## Problem

```css
/* www/index.html:229-230 — current */
#yogaOverlay .closeRow button { background: rgba(255,255,255,0.15); color: #fff; border: none; border-radius: 999px; width: 36px; height: 36px; font-size: 1rem; cursor: pointer; flex-shrink: 0; transition: transform 0.1s ease; }
#yogaOverlay .closeRow button:active { transform: scale(0.9); }
```

`scale(0.9)` is outside the subtle press-feedback range this codebase's own
audit standard specifies (0.95–0.98). It is also inconsistent with the sibling
control buttons two rules below in the same overlay:

```css
/* www/index.html:252-253 — current, for comparison */
#yogaOverlay .btnRow button { flex: 1; padding: 12px; border-radius: 12px; font-family: inherit; font-size: 0.9rem; font-weight: 600; cursor: pointer; border: none; transition: transform 0.1s ease; }
#yogaOverlay .btnRow button:active { transform: scale(0.96); }
```

Tapping the close (✕) button visibly "punches in" noticeably harder than
tapping Pause/Neste right below it in the same screen, despite both being
ordinary button presses with no reason to differ in weight.

## Target

```css
/* target — www/index.html:230 */
#yogaOverlay .closeRow button:active { transform: scale(0.96); }
```

## Repo conventions to follow

- `#yogaOverlay .btnRow button:active { transform: scale(0.96); }`
  (`www/index.html:253`) is the exemplar to match exactly — same overlay, same
  interaction weight, same value.
- This codebase's other press-feedback rules also sit in the 0.95–0.98 band:
  `.card:active { transform: scale(0.985); }`, `.btn:active { transform: scale(0.96); }`,
  `.goalBtn:active { transform: scale(0.96); }`. `0.96` keeps this button
  consistent with the majority convention already established across the file.

## Steps

1. Open `www/index.html`, locate `#yogaOverlay .closeRow button:active`
   (search for `.closeRow button:active` — note this selector pattern is
   shared verbatim by `#breathOverlay`, `#yogaOverlay`, and `#ritualOverlay`,
   each as its own rule; only change the `#yogaOverlay`-prefixed one).
2. Change:
   ```css
   #yogaOverlay .closeRow button:active { transform: scale(0.9); }
   ```
   to:
   ```css
   #yogaOverlay .closeRow button:active { transform: scale(0.96); }
   ```

## Boundaries

- Do NOT change `#breathOverlay .closeRow button:active` or
  `#ritualOverlay .closeRow button:active` — both currently also use
  `scale(0.9)` and are outside this plan's scope (yoga only, per the audit
  request). If asked to fix all three, that is a separate, trivially similar
  follow-up plan.
- Do NOT change the transition duration/easing on this rule (`0.1s ease`) —
  that is covered separately by Plan 004.
- Do NOT touch `#yogaOverlay .btnRow button:active` — it is already correct
  and is the reference value for this fix.
- If `www/index.html:229-230` does not match the excerpt above (drift since
  commit `78d086f`), STOP and report instead of guessing.

## Verification

- **Mechanical**: none (static file, no build step). Confirm the page still
  loads with no console errors.
- **Feel check**: open a yoga routine, tap-and-hold the ✕ close button and
  separately tap-and-hold the Pause button. Confirm both now compress by the
  same, subtle amount — neither should look visibly "heavier" than the other.
  In DevTools, set the Animations/Rendering panel to slow down interactions if
  needed, or simply compare the two buttons' computed `transform` at
  `:active` — both should read `scale(0.96)`.
- **Done when**: `#yogaOverlay .closeRow button:active` and
  `#yogaOverlay .btnRow button:active` specify the identical `scale(0.96)`
  value.
