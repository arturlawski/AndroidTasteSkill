---
name: design-android-ui
description: Design, audit, redesign, and implement polished native Android UI/UX with Jetpack Compose and Material 3. Use for Android screens, flows, components, design systems, prototypes, Compose code, UI reviews, responsive phone/tablet/foldable layouts, accessibility, interaction states, or requests to make an Android app feel less generic and more production-ready. Also use when translating Figma, screenshots, product briefs, or existing Android XML/Compose interfaces into coherent Android experiences.
---

# Design Android UI

Create Android-native product experiences with strong visual judgment. Preserve product clarity, accessibility, platform behavior, and engineering reality while eliminating template-like output.

## Start with a product read

Before designing or editing, inspect the brief and state one concise line:

`Reading this as: <product and core job> for <audience>, with <visual character>, optimized for <primary device/context>.`

Infer:

- product type and the user's primary job;
- audience, environment, frequency, and urgency of use;
- existing brand, screenshots, Figma, app structure, and technical stack;
- target form factors: phone, tablet, foldable, desktop window, or several;
- accessibility, privacy, regulated-industry, offline, and localization constraints.

Ask one clarifying question only when competing interpretations would materially change the flow. Otherwise proceed and label assumptions.

## Set three design dials

Choose and state values from 1 to 10:

- `EXPRESSIVENESS`: 1 is utilitarian and quiet; 10 is highly branded and art-directed.
- `MOTION`: 1 is nearly static; 10 is cinematic and gesture-rich.
- `DENSITY`: 1 is spacious and focused; 10 is information-dense.

Use `5 / 4 / 5` as a neutral baseline. Adjust from context, not taste alone. Accessibility, safety, task speed, and content volume override visual ambition.

## Choose the operating mode

### Greenfield design

Map the user journey before drawing screens. Define entry, primary task, success, recovery, and return path. Establish tokens and navigation, then implement representative screens and all important states.

### Existing app redesign

Inspect the repository before proposing changes. Identify Compose versus Views, Material version, theme and tokens, navigation, reusable components, dependencies, minimum SDK, and current behavior. Produce a short audit, prioritize high-impact fixes, and modify the existing stack incrementally. Do not migrate frameworks unless explicitly requested.

### Screenshot or Figma translation

Extract structure, hierarchy, spacing rhythm, typography roles, colors, components, states, and adaptive behavior. Reconstruct intent rather than blindly copying pixels. Replace non-native interaction patterns with Android-native behavior unless exact visual parity is the explicit goal.

### UX review only

Do not edit code. Report findings by severity and attach each issue to a screen, flow, component, or user outcome. Distinguish defects, accessibility failures, consistency problems, and optional polish.

## Design the flow before the surface

For multi-screen work, define:

1. user goal and entry point;
2. shortest successful path;
3. navigation model and back behavior;
4. loading, empty, partial, offline, error, permission-denied, and success states;
5. destructive-action confirmation and undo strategy;
6. state preservation after rotation, resizing, process recreation, and returning from another app.

Avoid screens that exist only to look impressive. Every surface must support a user decision or action.

## Use Material 3 as a foundation, not a costume

- Prefer Material 3 components, tokens, semantics, and interaction behavior for Compose projects.
- Theme `ColorScheme`, typography, shapes, spacing, and motion coherently. Do not ship unchanged sample-theme styling.
- Use dynamic color when it fits the product and brand. Provide a deliberate fallback palette and allow strict brands to opt out.
- Prefer platform components for navigation, sheets, dialogs, menus, snackbars, selection, text input, and system integration.
- Create a custom component only when standard behavior cannot express the product need. Preserve roles, semantics, focus, state, and touch feedback.
- Use one icon family. Prefer Material Symbols for native cohesion unless the product already has a licensed custom set.

Read [references/android-ui-guidelines.md](references/android-ui-guidelines.md) when selecting Android components, adaptive patterns, accessibility constraints, or verification steps.

## Apply anti-generic visual direction

Avoid default AI patterns unless the brief justifies them:

- a gradient header followed by repeated rounded cards;
- excessive floating containers and shadows;
- every control rendered as a pill;
- purple/blue glow as automatic “tech” branding;
- oversized greeting text that pushes the task below the fold;
- decorative charts with implausible data;
- a bottom bar with too many equal-priority destinations;
- generic onboarding slides that delay value;
- random illustration styles, emojis, or icon families;
- web landing-page composition squeezed into a phone viewport.

Create distinction through one or two controlled moves: a recognizable type scale, distinctive content framing, a purposeful color role, branded imagery, an unusual but usable composition, or a product-specific interaction. Do not add novelty everywhere.

## Establish hierarchy and tokens

Define semantic tokens before styling many screens:

- color roles for background, surface, text, outline, accent, status, and scrims;
- typography roles with scalable text and clear numeric hierarchy;
- spacing rhythm based on a small consistent scale;
- shape roles tied to component purpose;
- elevation used only to communicate layering or interaction;
- motion durations and easing by interaction category.

Keep contrast, radius, icon stroke, and surface treatment consistent. Prefer grouping through spacing and dividers before adding another card.

Use real, contextual draft copy and believable data. Avoid lorem ipsum, “Acme”, round vanity metrics, and vague AI marketing language.

## Design Android-native interaction

- Make the primary action obvious without turning every screen into a single giant CTA.
- Respect predictable system Back behavior and gesture navigation.
- Use edge-to-edge layout deliberately and handle system bars and insets.
- Keep bottom actions clear of gesture areas, the IME, and transient system UI.
- Preserve scroll position, selection, draft input, and navigation state.
- Use haptics only for meaningful confirmation, boundaries, or direct manipulation.
- Prefer inline validation and recovery. Use snackbars for brief, non-blocking feedback; dialogs only for decisions that truly interrupt the flow.
- Do not request permissions before the user understands the feature benefit. Handle denial and “don't ask again” states.
- Support keyboard, mouse, trackpad, focus navigation, and hover where larger Android devices make them relevant.

## Make layouts adaptive

Design from the current app window, not hardcoded device names or orientation.

- Use window size classes and Material 3 Adaptive patterns when available.
- Recompose information architecture for larger windows; do not merely stretch phone content.
- Consider list-detail and supporting-pane layouts for expanded widths.
- Adapt navigation among bottom navigation, rail, and drawer based on available space and destination count.
- Constrain readable content widths and avoid empty oceans around a narrow phone column.
- Account for fold posture, hinges, multi-window resizing, landscape, IME, and font scaling.

Provide at least compact and expanded previews for important screens. Add medium or foldable states when the flow changes materially.

## Build accessibility into the component

- Use semantic Material/Compose components before custom drawing.
- Keep interactive targets at least 48dp even when the visible icon is smaller.
- Provide meaningful roles, labels, state descriptions, headings, traversal order, and live feedback.
- Do not encode meaning through color alone.
- Support large font and display scaling without clipping, overlap, or hidden actions.
- Respect reduced motion and avoid motion essential to comprehension.
- Label meaningful images; hide purely decorative graphics from accessibility services.
- Test with TalkBack, Switch Access or keyboard navigation, high contrast where applicable, and automated accessibility checks.

Accessibility is a shipping constraint, not a final polish pass.

## Implement with Jetpack Compose

When code is requested:

- inspect version catalogs and Gradle files before importing libraries;
- use the project's existing architecture, theme, navigation, state holders, and dependency injection;
- hoist state and expose immutable UI state plus events; keep reusable composables stateless where practical;
- separate screen orchestration from reusable presentation components;
- use stable keys for lazy collections and avoid expensive work during composition;
- provide previews for meaningful states and window widths;
- add semantics and test tags intentionally, not indiscriminately;
- avoid hardcoded colors and repeated magic dimensions when semantic tokens exist;
- use vector drawables or licensed assets; never invent inaccessible icon-button glyphs from text characters;
- preserve existing behavior and test affected flows.

If the app uses XML Views, work within Views and Material Components unless migration is explicitly requested. Apply the same UX, adaptive, and accessibility standards.

## Handle motion with restraint

Motion must explain hierarchy, state change, navigation, or direct manipulation.

- Prefer short state transitions and shared spatial logic.
- Keep repeated interactions faster than first-time reveals.
- Avoid animating every list item on every return.
- Animate transform, alpha, bounds, or container transitions with performance in mind.
- Make gestures interruptible and reversible.
- Provide reduced or removed motion when requested by system/user preferences or when motion causes discomfort.

## Audit in priority order

For redesigns, fix in this order:

1. broken flow, unclear action, lost state, or unsafe behavior;
2. accessibility, contrast, touch targets, semantics, and text scaling;
3. navigation, back behavior, insets, keyboard, and adaptive layout;
4. hierarchy, content, spacing, typography, and component consistency;
5. loading, empty, offline, error, permission, and success states;
6. brand distinction, imagery, motion, and micro-polish.

Do not spend the budget on visual novelty while core UX remains broken.

## Verify before shipping

Check:

- the primary task is obvious and completable;
- every action has pressed, disabled, loading, success, and failure behavior where relevant;
- Back, deep links, rotation, resize, process restoration, and offline recovery behave coherently;
- compact and expanded layouts work with long localized copy and large fonts;
- system bars, cutouts, gesture navigation, and IME do not cover content;
- touch targets, contrast, semantics, focus order, and TalkBack output are usable;
- icons, type, colors, radii, and spacing follow one system;
- no placeholder content, dead controls, fabricated dependencies, or debug artifacts remain;
- Compose previews, UI tests, accessibility checks, lint, and the project's relevant tests pass.

Report what was changed, what was verified, and any remaining assumptions.
