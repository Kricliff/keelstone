---
name: Keelstone
description: Mental training and performance psychology for athletes, students, and leaders under pressure.
colors:
  deep-slate-teal: "#2F4F4F"
  deep-slate-teal-pressed: "#1f3636"
  teal-wash: "#e4ecea"
  compass-gold: "#D9B25C"
  gold-wash: "#fbf0dc"
  page-canvas: "#d6dbd9"
  surface: "#f9f9f9"
  card: "#ffffff"
  ink: "#1a1a1a"
  ink-soft: "#5f6f6f"
  hairline: "#e5e3dd"
  cat-focus: "#2a78d6"
  cat-calm: "#1baf7a"
  cat-confidence: "#4a3aa7"
  cat-restitution: "#e34948"
  danger: "#b9503c"
typography:
  headline:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "1.45rem"
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: "-0.01em"
  title:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "1.05rem"
    fontWeight: 600
    lineHeight: 1.3
  lead:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "0.9rem"
    fontWeight: 400
    lineHeight: 1.5
  body:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "0.85rem"
    fontWeight: 400
    lineHeight: 1.4
  label:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "0.76rem"
    fontWeight: 700
    letterSpacing: "0.04em"
  stat:
    fontFamily: "Manrope, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "1.5rem"
    fontWeight: 700
    letterSpacing: "-0.01em"
rounded:
  xs: "6px"
  sm: "8px"
  md: "10px"
  lg: "12px"
  xl: "16px"
  pill: "999px"
  sheet: "20px 20px 0 0"
spacing:
  xs: "6px"
  sm: "10px"
  md: "14px"
  lg: "18px"
  xl: "20px"
components:
  button-primary:
    backgroundColor: "{colors.deep-slate-teal}"
    textColor: "#ffffff"
    rounded: "{rounded.lg}"
    padding: "11px 18px"
  button-primary-active:
    backgroundColor: "{colors.deep-slate-teal-pressed}"
    textColor: "#ffffff"
    rounded: "{rounded.lg}"
    padding: "11px 18px"
  button-sand:
    backgroundColor: "{colors.compass-gold}"
    textColor: "#ffffff"
    rounded: "{rounded.lg}"
    padding: "11px 18px"
  button-secondary:
    backgroundColor: "{colors.teal-wash}"
    textColor: "{colors.deep-slate-teal-pressed}"
    rounded: "{rounded.lg}"
    padding: "11px 18px"
  button-ghost:
    backgroundColor: "transparent"
    textColor: "{colors.deep-slate-teal-pressed}"
    padding: "8px 10px"
  card:
    backgroundColor: "{colors.card}"
    rounded: "{rounded.xl}"
    padding: "18px"
  chip:
    backgroundColor: "{colors.card}"
    textColor: "{colors.ink-soft}"
    rounded: "{rounded.pill}"
    padding: "7px 14px"
  chip-active:
    backgroundColor: "{colors.deep-slate-teal}"
    textColor: "#ffffff"
    rounded: "{rounded.pill}"
    padding: "7px 14px"
---

# Design System: Keelstone

## Overview

**Creative North Star: "The Quiet Compass"**

Keelstone reads as an instrument you consult in a high-stakes moment, not a wellness app you browse when idle. The palette does the pointing: a grounded, unremarkable teal covers nearly everything, and a single warm gold appears only where it means something, a streak, an active moment, the orb you breathe with. Nothing shouts. The interface stays quiet on purpose so the one gold signal, or the one line of coaching text, is what you actually notice.

The system is calm and grounded rather than clinical. Cards sit on a soft neutral canvas with gentle two-layer shadows, never stark white-on-white and never a hard drop shadow. Corners are consistently rounded (16px cards, 12px buttons, full pills for badges and chips), and every interactive press scales down rather than lifting up, an understated, physical acknowledgment rather than a flourish. One typeface, Manrope, carries the entire hierarchy through weight alone.

This restraint is deliberate: the same coach who wrote every exercise script is, in effect, the same hand that drew every icon, hand-drawn single-stroke line marks rather than a licensed icon set. Nothing in the system feels licensed or generic; it feels like one person's tool, kept in your pocket.

**Key Characteristics:**
- Deep slate teal as the dominant, almost invisible color; warm gold reserved for the few things that deserve attention
- Soft, ambient two-layer shadows on card surfaces only; buttons and chips stay flat and use press-scale instead of elevation
- Fully mirrored light/dark themes (not an afterthought palette swap, every token has a considered dark counterpart)
- One typeface (Manrope) for the entire hierarchy, differentiated by weight and size, not by font pairing
- Hand-drawn single-stroke line icons (1.8px stroke, round caps/joins, currentColor), plus a signature concentric-rings mark used as logo, favicon, and the breathing orb's visual root

## Colors

Overwhelmingly neutral, with two accent colors used for two different jobs: teal carries the interface, gold carries meaning.

### Primary
- **Deep Slate Teal** (`#2F4F4F`): the app's entire chrome, primary buttons, header wordmark, active nav state, active chip/segment state, chart fills. This is the color of the app, not an accent.
- **Deep Slate Teal, pressed** (`#1f3636`): the `:active` state for primary buttons and the tint for secondary-button text; also the base of the header gradient on hero cards.

### Secondary
- **Compass Gold** (`#D9B25C`): reserved for signal, not decoration, the streak badge, the active state of onboarding/yoga progress dots, the breathing orb's glow, and the `sand` button variant used for the single most important action on a screen (start an exercise, confirm a destructive-adjacent choice).

### Category Accents (data/wayfinding only, never brand color)
- **Focus** (`#2a78d6`) · **Calm** (`#1baf7a`) · **Confidence** (`#4a3aa7`) · **Restitution** (`#e34948`): used exclusively to color-code the four exercise categories in progress bars and category chips. They never appear on buttons, headers, or brand marks; if a screen needs a "what category is this" signal, one of these four is correct, otherwise none of them are.

### Neutral
- **Page Canvas** (`#d6dbd9`): the backdrop behind the app's phone-width column on wider viewports; visible edge, not a working surface.
- **Surface** (`#f9f9f9`): the app's working background.
- **Card** (`#ffffff`): every card, sheet, input, and chip surface.
- **Ink** (`#1a1a1a`) / **Ink Soft** (`#5f6f6f`): primary and secondary text.
- **Hairline** (`#e5e3dd`): borders, dividers, track backgrounds (progress bars, switches).

Dark mode is a fully considered second set, not a filter: Deep Slate Teal lightens to a desaturated `#7fa39c` so it still reads as "teal" against a near-black `#141918` surface, and the category accents brighten to stay legible without turning neon.

**Because Deep Slate Teal and Compass Gold flip brightness in dark mode (both lighten, to stay legible against the near-black surface), text sitting directly on top of them cannot use a fixed white or fixed dark color across both themes** without failing WCAG AA (measured as low as 1.8:1 for white-on-gold, 2.8:1 for white-on-teal-in-dark-mode). Two utility tokens carry this: `--on-blue` (white in light mode, near-black `#141918` in dark mode, 6.4–8.9:1 either way) and `--on-accent` (a fixed near-black `#141918`, since gold never gets dark enough in either theme for white text to clear 4.5:1). Any new component placing text directly on a `--blue` or `--sand` fill must use these, not a literal white.

### Danger
- **Danger** (`#b9503c`): destructive-confirmation buttons only (delete account, replace-on-restore). Fixed across themes; white text on it clears AA (4.91:1) in both, so it doesn't need an on-color counterpart the way teal and gold do.

### Named Rules
**The Rare Gold Rule.** Compass Gold appears in only a handful of places per screen, ideally one. Its rarity is what makes it register as "this matters" rather than decoration. If a screen wants a second warm accent, that's a sign to use Deep Slate Teal instead, not to spend more gold.

**The On-Color Rule.** Never write `color: #fff` or `color: #000` directly on a `--blue` or `--sand` background. Use `--on-blue` / `--on-accent` so the text stays legible when the surface's brightness flips between themes.

## Typography

**Body & Display Font:** Manrope (400/500/600/700/800), with `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif` fallback.

**Character:** One typeface doing every job. Hierarchy comes from weight and size, never from switching families, so the interface never feels like it's borrowing a second voice for headlines.

### Hierarchy
- **Headline** (700, 1.45rem, -0.01em tracking): screen titles ("Profile", "Explore"). One per screen, at the very top.
- **Title** (600, 1.05rem): section headers within a screen (e.g. "Your data", "Weekly reflection").
- **Lead** (400, 0.9rem): the one-line explainer directly under a headline (`.sub`), and other single, slightly-emphasized supporting lines.
- **Body** (400, 0.85rem, 1.4 line-height): the actual workhorse size, exercise scripts, card descriptions, form helper text, secondary labels. This is the most-used text size in the app; if new secondary text doesn't obviously belong at Lead or Label, it's Body.
- **Label** (700, 0.76rem, uppercase, 0.04em tracking): card eyebrows and category tags, always small and quiet, never competing with the title above it.
- **Stat** (700, 1.5rem, -0.01em tracking): large numerals only, the weekly-ring count, stat-box values, mastery-log numbers. Never body copy at this size.

### Named Rules
**The One Typeface Rule.** Manrope covers headline through label. Do not introduce a second family, including for numerals or a "display" moment; weight and size already carry the full range this system needs.

**Known drift to clean up, not a pattern to copy.** A handful of meta/caption spots (category-bar labels, segmented-control labels, a few badge captions) still sit at 0.78–0.82rem instead of the documented 0.76rem Label or 0.85rem Body. Fold new instances into Label or Body directly; don't add a third micro-size to bridge the gap. The Flow Overlay's countdown numerals (up to 2.6rem) and the exercise-icon tile's emoji size (46px) are deliberate exceptions scoped to those two signature components, not part of the shared scale.

## Layout

Mobile-first single column, capped at `max-width: 460px` and centered against the Page Canvas backdrop, this holds even on desktop/web so the product always presents as a phone-width tool, never a responsive marketing layout. `env(safe-area-inset-*)` is respected everywhere real: header top padding, main bottom padding, nav bottom padding, modal sheet bottom padding, splash and overlay padding.

Content padding is `20px 18px` at the main level, with `14px` vertical rhythm between cards and `18px` internal card padding. A bottom tab bar (4 items: Home, Explore, Progress, Profile) is fixed and translucent (`backdrop-filter: blur(20px) saturate(180%)`), sitting above `env(safe-area-inset-bottom)`; main content reserves `90px` of bottom clearance so the last card never sits under the bar.

## Elevation & Depth

Soft, ambient two-layer shadows, never a hard single drop shadow, and never used on interactive elements at rest. The recipe is consistent everywhere it appears: `0 1px 3px rgba(31,54,54,0.06), 0 4px 14px rgba(31,54,54,0.09)` in light mode (a black-tinted equivalent in dark mode). It appears on cards, stat boxes, and the manus/script reading surface, places you read or scan, not places you press.

Buttons, chips, nav icons, and switches carry no shadow at all; their "pressed" feedback is a `scale(0.96)` (buttons) or `scale(0.985)` (cards) transform, never a shadow change. Depth in this system is about resting weight, not interactive lift.

### Shadow Vocabulary
- **Ambient card shadow** (`0 1px 3px rgba(31,54,54,0.06), 0 4px 14px rgba(31,54,54,0.09)`): the primary shadow in the system. Used identically on every card-level surface.
- **App shell shadow** (`0 0 30px rgba(0,0,0,0.15)`): a second, distinct role, the soft drop shadow the entire phone-width `#app` column casts against the Page Canvas on wider viewports. It reads as depth for the whole device frame, not a card, so it stays a separate, wider, more diffuse recipe rather than reusing the ambient card shadow.

### Named Rules
**The No-Lift Rule.** Interactive elements never elevate on press; they scale down. Reserve shadow entirely for static, resting surfaces.

## Shapes

Rounded and soft throughout, with corner radius scaling by role rather than by a single global value: `16px` for cards and the reading surface, `12px` for buttons and the exercise-icon tile, `10px` for inputs and mood buttons, `8px` for small icon containers (nav active state, brand mark, segmented control), `6px` for thin progress-bar tracks and fills (the weekly ring's linear cousins: `.playerBar`, category bars), and full pill (`999px`) for anything that represents state or status: chips, category segments, badges, the notification switch.

Bottom sheets (the modal, breath/yoga/ritual overlays' close affordance) round only their top corners (`20px 20px 0 0`), anchoring them visually to the screen edge they slide up from. Full-screen "flow" overlays (breathing, yoga, ritual) carry no radius at all; they occupy the entire viewport and intentionally break from the card grammar to signal "you have left the dashboard."

## Components

### Buttons
- **Shape:** `12px` radius, `11px 18px` padding, `600` weight text.
- **Primary:** Deep Slate Teal background, `--on-blue` text (white in light mode, near-black in dark mode, see the On-Color Rule); press state darkens to the pressed teal and always shows white text, since pressed teal stays dark in both themes.
- **Sand:** Compass Gold background, `--on-accent` text (fixed near-black). Reserved for the single most important action per the Rare Gold Rule (start an exercise, confirm a purchase/account action).
- **Secondary:** Teal Wash background with pressed-teal text, for a supporting action beside a primary.
- **Ghost:** Transparent background, pressed-teal text, tighter `8px 10px` padding, for dismiss/cancel actions that shouldn't compete visually.
- **Danger:** fixed danger-red background, white text always (see Colors). Destructive confirmations only, delete account, replace-on-restore; never a first action, always reached from behind a confirm step.
- **Small / Full:** modifiers, not separate components, reduce padding/font-size or stretch to `width: 100%`.

### Chips & Segments
- **Style:** white/card background, `1px` hairline border, pill radius, ink-soft text.
- **Active state:** fills solid Deep Slate Teal with `--on-blue` text and drops the border, the same active language as nav icons and goal buttons.

### Tool Row
- **Style:** a full-width, borderless `<button>` row reusing the exercise-list icon-tile grammar (`.exIcon`, Teal Wash `10px` tile + icon), a two-line label (bold title, Body-size ink-soft description), and a trailing chevron. Rows stack inside one card separated by a `1px` hairline, no radius or shadow of their own, the same "list inside one card" grammar as `.toggleRow` in Profile.
- **When to use:** grouping 3+ secondary tools or settings that don't warrant a full card and a headline each, the way Ritual/Goal/Reframe are grouped under "More tools" on Home. This is the system's answer to card sprawl: collapse into one Tool Row list before adding a fourth stacked card to a screen.

### Cards / Containers
- **Corner Style:** `16px` radius, uniform across the system.
- **Background:** white (`card` token) in light mode.
- **Shadow Strategy:** the single Ambient card shadow, see Elevation & Depth.
- **Hero variant:** a diagonal Deep Slate Teal → pressed-teal gradient with white text, used once per relevant screen for the "you have Pro" / primary status card, never for ordinary content cards.
- **Internal Padding:** `18px`.

### Inputs / Fields
- **Style:** `1px` hairline border, `10px` radius, card-colored background.
- **Focus:** border shifts to Deep Slate Teal with a `2px` Teal Wash outline glow, no shadow.

### Navigation
- Fixed bottom tab bar, translucent/blurred background, 4 flat icon+label buttons. Inactive icons are ink-soft; the active icon and label turn pressed-teal, and the active icon sits on a small Teal Wash pill (`8px` radius) behind it, the only "selected" treatment in the system that uses a background fill on an icon.

### Flow Overlay (signature component)
Full-screen takeover for guided breathing, yoga, and ritual sessions: a radial gradient from Deep Slate Teal to pressed-teal fills the entire viewport (`no card, no radius`), with a pulsing Compass Gold orb or hand-drawn glyph at the center as the pacing element, count and phase text in white, and a two-button control row (`sand` primary action, translucent-white secondary) pinned above the safe area. This is the moment the Quiet Compass metaphor is most literal: the instrument fills the screen and the gold element is the only thing that moves with intention.

## Do's and Don'ts

### Do:
- **Do** keep Compass Gold rare, one clear focal use per screen, per the Rare Gold Rule.
- **Do** use the four category colors only for category wayfinding (progress bars, category chips), never as a substitute brand color.
- **Do** give every new component a considered dark-mode counterpart at the same time as the light one; this system has never treated dark mode as generated-later.
- **Do** use scale-based press feedback (`0.96` buttons, `0.985` cards) for every interactive element; it is the system's only "you pressed this" language.
- **Do** write user-facing copy without em dashes (a durable product/brand commitment carried from PRODUCT.md).
- **Do** use `--on-blue` / `--on-accent` for any text sitting directly on a `--blue` or `--sand` fill, per the On-Color Rule.
- **Do** collapse a screen's third-and-later secondary tool into a Tool Row list instead of adding another full stacked card; Home should have exactly one primary action (the recommended exercise or the mood-driven branch) visible without scrolling past competing gold CTAs.
- **Do** use `0.1s` for press-feedback scale transforms and `0.16s` for background/color state transitions (hover-adjacent, toggled). These were consolidated from a 0.1-0.18s spread; don't introduce a third micro-duration.

### Don't:
- **Don't** add a second typeface. Manrope's weight range already covers headline through label.
- **Don't** put a shadow on an interactive element at rest (buttons, chips, nav, switches); shadow is reserved for static reading/scanning surfaces.
- **Don't** give the Flow Overlay (breath/yoga/ritual) a card radius or a bounded width; it is meant to feel like leaving the dashboard entirely, not like a bigger card.
- **Don't** show more than one Compass Gold button on screen at once; if a second situational action needs prominence, it goes in Secondary style, not a second Sand button.
- **Don't** fork visual treatment by audience (athlete vs. student vs. leader); Keelstone's product principle of one shared experience for all three groups extends to the visual system too.
