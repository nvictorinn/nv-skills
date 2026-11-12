---
name: ux-fix
description: Audit and fix the UX of a screen, flow or component. Use when building, reviewing or polishing how a UI behaves - loading, empty, error, success and partial states, forms, buttons, tap targets, accessibility, edge cases (offline, double submit, expired session), navigation, risky actions, mobile, UI copy (button labels, error and empty-state text), conventions, too many choices. Also when the user says "UX", "friction", "users get confused" or "add the missing states". Checks the unhappy paths AI tools usually skip.
argument-hint: "[target] [topics...] [all] [short|long]"
---

# ux-fix

Find where a UI fails the user, then fix it. Visual style is out of scope, except where it affects use (contrast, focus, target size). Detail and numbers: [references/rules.md](references/rules.md).

## Scope

Arguments are optional and combine a **target** (file, folder, screen or flow) with **topics**:

- `/nv-skills:ux-fix` or `/nv-skills:ux-fix all`: full audit of what the conversation is about. If that is unclear, ask which flow.
- `/nv-skills:ux-fix src/app/recipe/edit.tsx`: full audit of that target.
- `/nv-skills:ux-fix forms` or `/nv-skills:ux-fix checkout errors, a11y`: only those topics.
- Add `short` anywhere (`/nv-skills:ux-fix short`, `/nv-skills:ux-fix forms short`) for the condensed report. Accept "summary" or "synthétique". Detailed is the default; `long`, "detailed" or "détaillé" ask for it explicitly.

Topics: `states`, `loading`, `partial`, `errors`, `empty`, `success`, `forms`, `buttons`, `targets`, `conventions`, `choices`, `complexity`, `copy`, `a11y`, `edge`, `navigation`, `mobile`. Accept close wording ("accessibility", "tap targets"). A word that is neither a topic nor an identifiable target: show this list instead of guessing.

Problems cross topics, so a topic audit is never a clean bill of health. List the other topics as not checked and name the closest ones to run next: `forms` -> `errors`, `navigation`, `edge`; `loading` -> `partial`, `errors`; `errors` -> `copy`, `edge`; `buttons` -> `loading`, `a11y`, `targets`; `empty` -> `states`, `copy`; `navigation` -> `edge`, `errors`; `edge` -> `errors`, `navigation`; `a11y` -> `targets`, `copy`; otherwise the two closest. If a topic has nothing to check on the target, say so in one line and suggest the nearest one.

## Method

1. List every screen/flow touched, its data sources and actions, and its platform (web, native, both). For a file target, also read what controls it (navigation config, parent layout, data source). For a whole-app audit, build the inventory before checking anything: every route from the navigation config, plus every sheet, menu, modal, dialog, settings sub-screen and permission screen, plus what can change outside the app (data edited or deleted elsewhere, permission revoked). Check every item or list it as not covered; never drop one silently. Read decisions already validated (handoff notes, specs, mockups). Do not propose reversing one without saying so.
2. Run the checklist on each, limited to the requested topics if any. Skip lines that do not apply, without listing them. Web only: hover, focus ring, tab order. Native equivalents: [references/examples.md](references/examples.md).
   **Whole-app audit: one run, in parallel.** One agent skims a big app, so split it when subagents are available, instead of asking for more passes:
   - First read once what all screens share (navigation, data loading, error handling, shared components) and write a shared map of 20 to 30 lines with `file:line`. Subagents use the map and do not re-read those files.
   - One subagent per group of 3 to 5 related screens from the inventory, all launched at once, on a cheaper model (`model: "sonnet"`). Each gets the Checklist section of this file pasted in, the shared map, the absolute paths of the reference files, its screens with their files, and the topics. It goes deep on its group only and returns one line per finding in the short format, with `file:line` and seen or code. The main agent keeps inventory, merge and ranking, and writes the full report itself.
   - One cross-cutting subagent, same model, map and return format, for what no single screen shows: day change or midnight on an open tab, data changed outside the app, offline and retry, session expiry, back, refresh and deep links between screens, import, export and reset of saved data.
   - Merge: the same issue on several screens becomes one finding that lists them all. Then one last subagent, also on the cheaper model, gets the inventory and the merged list, looks for what is still missing, and its findings join the same report.
   Without subagents, check the groups one after the other in this run. Never end with "run again for the rest".
3. Rank findings and report them in the format under Output. Severity: **blocker** (user cannot finish, any data loss, a user action silently not done, inaccessible), **friction** (user finishes but confused or slowed, including a wrong figure with no warning), **polish** (nice to have). A finding is rare when it needs an unusual situation. Sort: blockers, rare blockers, friction, rare friction, polish. Always show every blocker, rare or not, however many there are. Fill up to 10 with the next findings, say how many remain, and give the rest on request.
4. Propose by default. Apply fixes only when asked or when the task is "fix UX".
5. If the UI targets phones, test on a real or emulated device when you can: missed taps, scroll vs tap, one-handed reach. Note for each finding whether it was seen on screen or only read in the code.

## Checklist

**The four states.** Every screen that loads data has loading, success, error, empty. Generated code usually covers only success: check the other three exist. Not loaded yet is not empty: never show an empty or not-found state before the first result arrives.

**Loading.** Skeleton when the layout is known. Determinate progress when size or duration is known. Small in-place spinner for one control or region. Optimistic update only for cheap, reversible actions, restoring the previous value on failure; never for payments or irreversible actions. Delay indicators by about 300-500 ms to avoid flicker. Up to about 5 s a simple indicator, then say what is happening, past about 10 s real progress and a way to cancel. Report failure as soon as it is known.

**Partial states.** Each region loads and fails on its own. One broken widget never blanks the page.

**Errors.** Say what failed, why if it helps, and what to do now. Never show raw backend text (also a security leak). No generic failure message when the outcome matters. Never fail silently. Sign-in and password reset must not reveal whether an account exists. Placement: inline at the source by default, toast only for what is safe to miss, dialog only when work cannot continue, with the action that resolves it. The error must be visible from where the user acted: after a failed submit, scroll or move focus to it.

**Empty states.** Answer by cause: nothing yet (explain and give the starting button), no match (repeat the query, offer to widen it), all done (calm and positive), no access or failed load (treat as an error). Never a blank area.

**Success.** Every action gives a visible signal, sized to the action. Often the visible change is enough. Celebration only for rare moments.

**Forms.** Validate when leaving a field, not during first entry, and clear the error as soon as it is fixed. Show remaining characters when limited. Tick off password rules live. Prefill what is known. Accept natural input and normalize it, but never guess ambiguous values such as dates. Mark required fields when it is not obvious. Group long forms into steps. Client limits match backend constraints (length, type, required): read the schema or migrations. In repeated entry, Return goes to the next field and new rows take focus.

**Buttons.** States: default, hover (pointer only, `@media (hover: hover)`), focus (visible), pressed (immediate feedback), loading (inside the button), disabled. Prefer an enabled Submit that, pressed too early, shows what is missing and moves focus there. Disable only during an action in progress or when the reason is plain from context.

**Tap targets (Fitts).** Interactive area bigger than the icon, labels part of their control, gaps between adjacent actions, frequent actions in thumb reach. Tappable rows must not fire while scrolling.

**Conventions (Jakob).** Keep familiar patterns (logo goes home, magnifier means search). Respect platform differences and mirror for RTL locales. Be original where it helps the task, not in where things are.

**Choices (Hick).** Fewer options at one moment: defaults, groups, filters, steps. Progressive disclosure: reveal the rest on request, but keep the main feature visible.

**Complexity (Tesler).** Move complexity into the product: detect, parse, remember, default. Design for someone distracted and in a hurry.

**Microcopy.** Buttons lead with a specific verb that says the outcome ("Save changes", "Delete 3 files"), never bare "OK"/"Submit"/"Click here". Calm, blame-free, no jargon, one term per concept, in the product's language. Clarity beats cleverness at decision points. Templates for errors, empty states, success: [references/copy.md](references/copy.md).

**Accessibility.** Text contrast 4.5:1 (3:1 for large text and UI icons). Every input has a visible label (placeholder is not a label). On web, everything usable by keyboard, in logical tab order, with a visible focus ring; open modals keep focus inside until closed. On native, every control has a screen reader label that tells similar items apart. Never convey meaning by color alone. Alt text on meaningful images, none on decorative ones. Respect reduced motion and text scaling. Announce errors and status changes to screen readers.

**Edge cases.** Offline, slow network, timeout and retry. Double click or double submit. Expired session (keep the user's input). List of 0, 1 and 1000 items. Very long, short, accented or RTL text. Back button, refresh and deep link mid-flow. Offline-first: a rejected sync item is visible to the user and does not block the rest of the queue. Local state can diverge from the real source (item deleted elsewhere, file gone): reconcile when it is shown.

**Navigation and risky actions.** User always knows where they are and how to go back. Prefer undo over "Are you sure?"; if you confirm, name the consequence and use a specific button label. Make destructive actions visually distinct and never the default. Check every way out of a screen (button, back gesture, Android back, deep link): all go through the same guard. Do not lose in-progress input or pending selections. Once an irreversible action has started, exit and cancel controls must say what really happened.

**Mobile.** Right keyboard per field (email, number, tel), autofill hints on fields of a known type, field not hidden behind the keyboard, respect safe areas, no hover-only features, gestures always have a visible alternative.

## Output

Write for someone who has not read the code, in the language of the conversation and in full sentences, even if the session otherwise uses a terse style.

Start with two or three sentences: the overall state of the flow and what to fix first. Then one coverage line: `Covered: ... Not covered: ... (why).` A report without it reads as a clean bill of health it is not. Then the top findings:

**1. Title in everyday words** (Blocker, frequent, seen on screen)
- **What happens:** a concrete scene from the user's side: what they do, what they see.
- **Why it matters:** what they lose or misunderstand.
- **Fix:** what will change for the user, then the technical hint and `file:line`.

Name screens and buttons as the user sees them. Keep code names for the end of the Fix line. No arrows, no abbreviations, no unexplained jargon. End with the number of findings remaining and the topics not run.

**Short report:** for a quick read, so every part shrinks. One sentence of summary. Coverage as names only, no explanations: `Covered: A, B. Not covered: C (no device).` Then exactly one line per finding, about 30 words at most: `1. Title in everyday words (Blocker, frequent, seen on screen). What happens, in one sentence. Fix: what changes for the user (file:line).` No why-it-matters, no technical detail beyond file:line, no closing advice. End with one line: findings remaining, and that any number can be detailed. Give a finding in the full format when asked. Before/after code for the common fixes: [references/examples.md](references/examples.md).
