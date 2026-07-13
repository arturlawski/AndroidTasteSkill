# Android UI implementation reference

Load this reference when choosing components, implementing Compose UI, auditing accessibility, or planning adaptive behavior.

## Foundation

- Prefer Jetpack Compose and Material 3 for greenfield Android UI unless project constraints require Views.
- In existing apps, preserve the current UI toolkit and migrate only with explicit scope.
- Treat Material as behavioral and semantic infrastructure. Customize tokens and content hierarchy to create product character.
- Verify exact dependency versions and current APIs from the project's version catalog and official Android documentation before coding.

## Component selection

| Need | Default direction | Avoid |
| --- | --- | --- |
| 3–5 top-level destinations on compact width | Navigation bar | Scrollable row of equal tabs for unrelated destinations |
| More destinations or larger window | Navigation rail or drawer | Permanently stretched bottom navigation |
| Contextual primary creation action | FAB when it is truly dominant | FAB on every screen by habit |
| Temporary secondary action | Snackbar | Dialog or toast for recoverable inline feedback |
| Destructive or consequential decision | Alert dialog with explicit action | Confirmation for harmless actions; vague “OK” |
| Short selection set | Chips, segmented buttons, radio controls | Dropdown that hides a tiny set of choices |
| Long or searchable selection | Exposed dropdown/search surface | Huge chip cloud |
| Supporting actions | Bottom sheet or menu | Full-screen detour without need |
| List and details on wide windows | `ListDetailPaneScaffold` | Enlarged single phone list |
| Primary plus contextual support | `SupportingPaneScaffold` | Floating secondary card over content |
| Adaptive top-level navigation | `NavigationSuiteScaffold` | Device-model checks |

Use component APIs that preserve semantics and minimum target enforcement. When customizing visuals, retain interaction source, enabled/disabled behavior, focus, roles, and state descriptions.

## Adaptive layout

- Base layout decisions on current window metrics and window size classes.
- Compact: usually one pane and concise navigation.
- Medium: use available width for stronger grouping or navigation rail; do not assume two panes always improve comprehension.
- Expanded: consider simultaneous related panes, persistent supporting content, and constrained reading widths.
- Treat posture and hinge as layout features. Do not place critical controls beneath a fold or hinge.
- Preserve continuity across resize and configuration change through saved state and state holders.
- Preview and test representative compact, medium, expanded, landscape, foldable, and multi-window conditions relevant to scope.

## Edge-to-edge and input

- Consume insets at the correct layout boundary; avoid applying the same inset twice.
- Test status and navigation bar icon contrast in light and dark themes.
- Keep persistent bottom actions visible when the IME opens or intentionally scroll them with content.
- Support focus order, Enter/Space activation, directional navigation, hover, right-click/context actions, and scroll wheels when target devices warrant them.
- Never branch layout from `Build.MODEL`; respond to capabilities, window, posture, and input mode.

## Accessibility baseline

- Minimum interactive target: 48dp by 48dp.
- Keep text in `sp` through Compose typography; test large font and display scales.
- Provide a useful `contentDescription` for meaningful non-text content; use `null` for decorative imagery.
- Add role and state semantics to custom controls. Announce state changes that are not otherwise discoverable.
- Merge semantics only when the combined element is the meaningful interaction unit.
- Keep TalkBack traversal aligned with task order, not merely drawing order.
- Avoid duplicate announcements from an icon and its adjacent label.
- Pair every validation error with actionable text and a programmatic relationship to its field.
- Check contrast for text, icons, focus indication, charts, disabled states, and content over images.
- Offer non-gesture alternatives for swipe-only actions.

## UI state model

Represent meaningful states explicitly rather than scattering booleans:

```kotlin
sealed interface ScreenUiState {
    data object Loading : ScreenUiState
    data object Empty : ScreenUiState
    data class Content(val items: List<Item>) : ScreenUiState
    data class Error(val message: UiText, val retryable: Boolean) : ScreenUiState
}
```

Adapt the model to the project. Preserve existing content during refresh when possible; distinguish initial loading from background refresh and partial failure.

## Compose quality checklist

- Hoist state to the lowest common owner that needs it.
- Pass events upward; do not let reusable components own business logic.
- Collect lifecycle-aware flows using project-standard APIs.
- Use `rememberSaveable` only for suitable UI state; use a `ViewModel` or persistence for durable state.
- Give lazy items stable keys.
- Use `derivedStateOf` for derived values that would otherwise cause excessive recomposition.
- Avoid allocating brushes, formatters, or large collections on every recomposition.
- Keep composable parameters stable where it improves actual performance; do not annotate blindly.
- Supply previews for light/dark, loading/content/error, long text, large font, and relevant window sizes.
- Test semantics and user outcomes rather than internal composable structure.

## Visual QA matrix

Test the smallest useful matrix for the feature:

| Dimension | Representative checks |
| --- | --- |
| Theme | Light, dark, dynamic color/fallback where supported |
| Window | Compact portrait, compact landscape, expanded; medium/foldable if behavior changes |
| Text | Long localization, large font, long numbers, right-to-left if supported |
| State | Loading, content, empty, partial, error, offline, disabled, success |
| Input | Touch, TalkBack, keyboard/focus; mouse/trackpad where relevant |
| System UI | Gesture navigation, three-button navigation if supported, IME, cutout, edge-to-edge |

## Official references

Verify changing APIs against:

- Material 3 in Compose: https://developer.android.com/develop/ui/compose/designsystems/material3
- Adaptive apps: https://developer.android.com/develop/ui/compose/layouts/adaptive
- Accessibility in Compose: https://developer.android.com/develop/ui/compose/accessibility
- Accessibility API defaults: https://developer.android.com/develop/ui/compose/accessibility/api-defaults
- Accessibility testing: https://developer.android.com/develop/ui/compose/accessibility/testing
