---
name: ux-fix
description: Audit and fix the UX of a screen, flow or component. Use when building, reviewing or polishing how a UI behaves - loading, empty, error, success and partial states, forms, buttons, tap targets, accessibility, edge cases (offline, double submit, expired session), navigation, risky actions, mobile, UI copy (button labels, error and empty-state text), conventions, too many choices. Also when the user says "UX", "friction", "users get confused" or "add the missing states". Checks the unhappy paths AI tools usually skip.
---

# ux-fix

Find where a UI fails the user, then fix it. Visual style is out of scope, except where it affects use (contrast, focus, target size). Detail and numbers: [references/rules.md](references/rules.md).

## Method

1. List every screen/flow touched and its data sources and actions.
2. For each one, run the checklist below. Skip lines that do not apply.
3. Report findings as `[severity] place: problem -> fix`, worst first. Severity: **blocker** (user cannot finish, data loss, silent failure, inaccessible), **friction** (user finishes but confused or slowed), **polish** (nice to have). Fix them in code when asked or when the task is "fix UX"; otherwise propose.
4. If the UI targets phones, test on a real or emulated device: missed taps, scroll vs tap, one-handed reach.

## Checklist

**The four states.** Every screen that loads data has loading, success, error, empty. Generated code usually covers only success: check the other three exist.

**Loading.** Skeleton when the layout is known. Determinate progress when size or duration is known. Small in-place spinner for one control or region. Optimistic update only for cheap, reversible actions, restoring the previous value on failure; never for payments or irreversible actions. Delay indicators by about 300-500 ms to avoid flicker. Up to about 5 s a simple indicator, then say what is happening, past about 10 s real progress and a way to cancel. Report failure as soon as it is known.

**Partial states.** Each region loads and fails on its own. One broken widget never blanks the page.

**Errors.** Say what failed, why if it helps, and what to do now. Never show raw backend text (also a security leak). No generic failure message when the outcome matters. Never fail silently. Sign-in and password reset must not reveal whether an account exists. Placement: inline at the source by default, toast only for what is safe to miss, dialog only when work cannot continue, with the action that resolves it.

**Empty states.** Answer by cause: nothing yet (explain and give the starting button), no match (repeat the query, offer to widen it), all done (calm and positive), no access or failed load (treat as an error). Never a blank area.

**Success.** Every action gives a visible signal, sized to the action. Often the visible change is enough. Celebration only for rare moments.

**Forms.** Validate when leaving a field, not during first entry, and clear the error as soon as it is fixed. Show remaining characters when limited. Tick off password rules live. Prefill what is known. Accept natural input and normalize it, but never guess ambiguous values such as dates. Mark required fields. Group long forms into steps.

**Buttons.** States: default, hover (pointer only, `@media (hover: hover)`), focus (visible), pressed (immediate feedback), loading (inside the button), disabled. Prefer an enabled Submit that, pressed too early, shows what is missing and moves focus there. Disable only during an action in progress or when the reason is plain from context.

**Tap targets (Fitts).** Interactive area bigger than the icon, labels part of their control, gaps between adjacent actions, frequent actions in thumb reach. Tappable rows must not fire while scrolling.

**Conventions (Jakob).** Keep familiar patterns (logo goes home, magnifier means search). Respect platform differences and mirror for RTL locales. Be original where it helps the task, not in where things are.

**Choices (Hick).** Fewer options at one moment: defaults, groups, filters, steps. Progressive disclosure: reveal the rest on request, but keep the main feature visible.

**Complexity (Tesler).** Move complexity into the product: detect, parse, remember, default. Design for someone distracted and in a hurry.

**Microcopy.** Buttons lead with a specific verb that says the outcome ("Save changes", "Delete 3 files"), never bare "OK"/"Submit"/"Click here". Calm, blame-free, no jargon, one term per concept, in the product's language. Clarity beats cleverness at decision points. Templates for errors, empty states, success: [references/copy.md](references/copy.md).

**Accessibility.** Text contrast 4.5:1 (3:1 for large text and UI icons). Every input has a visible label (placeholder is not a label). Everything usable by keyboard, in logical tab order, with a visible focus ring; open modals keep focus inside until closed. Never convey meaning by color alone. Alt text on meaningful images, none on decorative ones. Respect reduced motion and text scaling. Announce errors and status changes to screen readers.

**Edge cases.** Offline, slow network, timeout and retry. Double click or double submit. Expired session (keep the user's input). List of 0, 1 and 1000 items. Very long, short, accented or RTL text. Back button, refresh and deep link mid-flow.

**Navigation and risky actions.** User always knows where they are and how to go back. Prefer undo over "Are you sure?"; if you confirm, name the consequence and use a specific button label. Make destructive actions visually distinct and never the default. Do not lose in-progress input on navigation.

**Mobile.** Right keyboard per field (email, number, tel), correct autocomplete attributes, field not hidden behind the keyboard, respect safe areas, no hover-only features, gestures always have a visible alternative.

## Output

Findings list with severity plus the patch. Before/after code for the common fixes: [references/examples.md](references/examples.md). Say which checks you skipped.
