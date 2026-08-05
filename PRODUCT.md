# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Three audiences, deliberately equal-weighted rather than segmented by role or track: athletes, students, and leaders/professionals, each facing performance-under-pressure situations (competition, exams, high-stakes meetings and decisions). The app does not split content or UI by audience; the same exercise library and copy serve all three. Bilingual by design: Norwegian and English, auto-detected from device language, not two separate apps.

## Product Purpose

Keelstone is a short-session mental training and performance psychology app: guided audio/text exercises (breathing, grounding, focus, reframing, visualization) organized into four categories (Fokus, Ro, Selvtillit, Restitusjon / Focus, Calm, Confidence, Restitution), plus yoga sessions. It exists to give people a concrete tool to use in the moments right before or after they need to perform, not a general wellness habit app.

## Positioning

Two things a generic mindfulness app could not truthfully claim:

1. **Coach-authored, not generic.** Every exercise script is written by Kristian Clifford, a working coach and mental trainer, for this specific audience — not crowdsourced or templated meditation content.
2. **Performance under pressure, not relaxation.** The mechanism is "the one thing to focus on right before/during a high-stakes moment," distinct from Calm/Headspace's sleep-and-general-stress framing.

## Operating Context

- Capacitor-wrapped web app; iOS live (TestFlight as of 2026-07-23), Android planned but not yet added.
- Used in short, situational bursts: before competition, before an exam, before/during a difficult meeting — not necessarily a daily ritual, though daily/weekly reminders exist.
- No login required to use the app; an optional email/password account (added 2026-08-01) lets progress sync across devices and survive reinstall, via Firebase.
- Distributed through the App Store; support and coaching funnel route back to cliffordcoaching.no.

## Capabilities and Constraints

- 20 guided exercises across 4 categories + yoga sessions, one free exercise per category, rest gated behind a single Pro monthly subscription (RevenueCat, 3-day free trial via App Store Connect introductory offer).
- Optional account (email/password) syncs a Firestore-backed data backup across devices; anonymous users get a same-device-only backup instead. Account deletion is self-service in-app (Apple Guideline 5.1.1(v)).
- Daily and weekly reminder notifications, scheduled entirely on-device (`@capacitor/local-notifications`); no analytics, ad SDKs, or cross-app tracking.
- Audio narration for exercises exists as scripts (`content/manus.md`) but is not yet recorded — currently text-only, read by the user while audio is pending.
- Not yet on Google Play; Android build support exists in Capacitor config but the store listing isn't live.

## Brand Commitments

Current, not yet formally locked (explicitly open to revision without prior approval as of this writing):

- Name: Keelstone. Color palette shares its base with cliffordcoaching.no — primary `#2F4F4F` (deep slate/teal), background `#f9f9f9`, near-black text, with `#D9B25C` gold as an added accent color (the coaching site itself has none). App icon/header mark: concentric teal/gold rings, a breathing-exercise motif.
- Typeface: Manrope (switched 2026-07-24 from PT Sans, deliberately diverging from cliffordcoaching.no's font for a cleaner, more professional feel).
- No em dashes in user-facing copy.
- Voice, evidenced by the exercise scripts in `content/manus.md`: calm, second-person, embodied and concrete ("notice where in your body..."), present-focused, coaching rather than clinical in register. Not yet written down as a formal style guide.

## Evidence on Hand

- 20 full exercise scripts (Norwegian + English) in `content/manus.md`, already live as in-app text.
- No user testimonials, reviews, usage data, or case studies yet — first TestFlight build shipped 2026-07-23; do not fabricate any of these for future work.

## Product Principles

1. One concrete action per exercise, not general advice — mirrors the "focus on the one thing in front of you" mechanism the scripts teach.
2. Same content for every audience; don't fork the app by role or segment.
3. Situational, short-session tool first; habit-building (streaks, reminders) is secondary scaffolding, not the core loop.
4. Optional, not required: account creation, reminders, and Pro are all opt-in on top of a working free/anonymous experience.
5. Bilingual parity: Norwegian and English content and timing stay matched, not English-first with a translated afterthought.
