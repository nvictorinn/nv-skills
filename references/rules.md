# UX rules

Durations and thresholds are rules of thumb. Measure on your own product when it matters.

## What to check first
A UI can look finished and still fail people. Ask: can someone work it out without instructions, and what happens when something goes wrong? Generated code tends to cover only the path where everything works, so the loading, empty and error paths are where most problems hide.

## Loading
Choose the indicator from what you know about the wait:
- Layout known, content pending (a list, a dashboard): skeleton matching the final layout.
- Duration or size known (file transfer, import, export): determinate progress with a percentage or count. An indeterminate spinner on a long transfer reads as frozen.
- One control or small region busy: small spinner in place, so the rest of the page stays usable.
- Cheap, reversible, very likely to succeed (star, toggle, reorder): optimistic update, restore the previous value on failure. Never for payments, irreversible actions, or anything the user must be sure really happened.

Timing:
- You rarely know the duration in advance, so delay the indicator by about 300-500 ms. Fast responses then show no flicker. Once shown, keep it visible long enough to read.
- The pressed control still reacts at once, even while the indicator is delayed.
- Up to about 5 s: a simple indicator is enough.
- Beyond that, say what is happening, ideally by stage ("Checking your file", "Importing 120 of 400 rows").
- Past about 10 s: real progress or steps, and let the user leave or cancel.
- As soon as a request has failed, say so.

## Partial states
Most screens combine several independent requests. Render each region as soon as it has data, and give each one its own loading and error state. A failed revenue chart shows a retry in its own card while the invoice table stays usable. Only block the whole page when nothing on it makes sense without that data.

Do not confuse "no data yet" with "no data". Many data hooks return an empty list on first render: wait for the first result before showing an empty or not-found state, and treat a failed first sync as an error, not as an empty account.

## Errors
- Never display raw exceptions, SQL or stack traces: users cannot act on them and they leak implementation details.
- A generic failure message is a dead end, worst when the user cannot tell whether the action happened (was the order placed?). If the outcome is unknown, say so and say how to check.
- Give the facts the user needs: what failed, why if it helps, what to do now. "Your file wasn't uploaded: it's over the 25 MB limit. Compress it or pick a smaller one."
- Exception for sign-in and password reset: do not reveal whether an account exists. Use the same message in both cases.
- A control that does nothing on failure is the most damaging case. Catch every failure path and surface it.
- Placement:
  - Inline, at the source: the default. A message under the invalid field, a retry beside the action that failed.
  - Toast: only for things that are safe to miss or that recover alone. Long enough to read, and never the only record of a failure.
  - Dialog: only when work cannot go on until it is resolved, and always with the action that resolves it (renew the plan, ask for access).

## Empty states
Find out why the area is empty, because each cause needs a different answer:
- Nothing created yet: explain what will appear here and give the button that starts it. This is often a new user's first impression.
- Filter or search matched nothing: repeat the query, then offer to clear filters, check spelling or widen the search.
- Everything done: a calm, positive message. It must not look like a fault.
- No permission or failed load: this is an error, treat it as one.

## Success
Every completed action needs a signal the user can see. Without one, people repeat the action, which is costly when it creates an order or sends a message. Size the signal to the action: often the visible change itself is enough (the row appears in the list). Reserve full pages and celebration for rare moments, such as finishing setup.

## Forms
1. Mark required fields, or mark the optional ones if most are required.
2. Validate a field when the user leaves it, not during the first entry. Once an error shows, clear it as soon as the value is valid.
3. Show remaining characters when there is a limit.
4. Prefill what you already know.
5. Show password rules up front and tick them off during typing.
6. Accept input as people naturally write it (spaces in a card or phone number) and normalize it yourself. Never guess ambiguous values: 03/04 can be two different dates, so use a date picker or show the interpreted date.
7. Long forms: group into steps and show progress. Keep what was entered when moving back.

## Buttons
States to design: default, hover, focus, pressed, loading, disabled.
- Hover exists only with a pointer: scope it with `@media (hover: hover)` so it does not stick after a tap.
- Focus must be clearly visible for keyboard users.
- Pressed gives immediate feedback, otherwise people press again.
- Loading sits inside the button, keeps its width, and blocks a second submit.

Disabled state: a greyed-out button says "no" without saying why. Keyboard and screen reader users may never reach it. Default to an enabled submit that, when pressed too early, shows what is missing and moves focus to the first problem. Disable when the action is in progress or the reason is plain from context (no earlier page exists). If the control must stay reachable, use `aria-disabled` and explain why. Test: would the user have to investigate how to enable it? Then do not disable it.

## Fitts's law and tap targets
The time to hit a target depends on its size and distance. Enlarge the interactive area beyond the visible icon, make labels part of their control, leave a gap between adjacent actions, keep frequent actions within thumb reach. Large tappable rows must still scroll cleanly: a scroll gesture must not count as a tap. Check on a real device.

## Jakob's law and platforms
People learn interfaces elsewhere and bring those habits with them. Keep the usual patterns: logo returns home, magnifier means search, underlined text is a link. Spend originality where it helps the task, not on where things are. Conventions differ by platform and locale: bottom navigation on phones, system back behavior on each OS, mirrored layouts for right-to-left languages.

## Hick's law
More options and harder comparisons mean slower decisions. Offer a sensible default, group related settings, keep plans comparable and few, move rare options behind filters or search. The goal is fewer choices at one moment, not less capability.

## Progressive disclosure
Show what the current step needs and reveal the rest on request: an "Advanced" section, a command palette, details on expand. Do not hide the main feature: if most users need it, it stays visible. Ask of every element: is it needed at this moment?

## Tesler's law
Every task has a complexity that cannot be removed, only moved between the product and the person using it. Move it into the product: detect the time zone, parse pasted data, remember the last choice, pick good defaults. Design for someone distracted and in a hurry.

## Accessibility
- Contrast: 4.5:1 for body text, 3:1 for large text and meaningful icons/borders.
- Labels: each input has a visible, programmatically linked label. A placeholder disappears when typing and is not a label.
- Keyboard: every action reachable and operable without a mouse, logical tab order, visible focus ring (never `outline: none` without a replacement). Focus never gets stuck, except inside an open modal, which keeps focus within it until closed. Escape closes overlays and focus returns to the trigger.
- Color: never the only signal (error = red border + icon + text).
- Images: meaningful ones get alt text describing the purpose; decorative ones get empty alt.
- Motion: honor `prefers-reduced-motion`; no auto-playing motion that cannot be paused.
- Text: layouts survive 200% zoom and larger system text.
- Dynamic updates: errors, toasts and loading status are announced (`aria-live`, `role="alert"`). Move focus to the first error on a failed submit.
- Touch targets at least about 44x44 pt (iOS) or 48x48 dp (Android).

## Edge cases and network
- Offline or flaky network: say so, keep what the user did, retry automatically or with a button.
- Timeout: set one, then show the error with a retry; do not spin forever.
- Double click / double submit: ignore while pending; make the backend idempotent for payments.
- Session expiry: preserve unsaved input, ask to sign in again, return to where they were.
- Data extremes: 0 items (empty state), 1 item (singular wording), 1000 items (pagination or virtualization, search), very long names (truncate with a way to see all), accents, emoji, right-to-left text.
- Interruptions: browser back, refresh, deep link into the middle of a flow, second tab. Nothing should break or silently lose data.

## Navigation and risky actions
- The user always knows where they are and how to go back; the back button does what they expect.
- Undo beats confirmation: do the action, offer "Undo" for a few seconds (archive, delete to trash). Confirmation dialogs get ignored by habit.
- When a confirmation is needed (irreversible, expensive, affects others), name the consequence and label the button with the action ("Delete project", not "Yes"). Destructive buttons look different and are never the default focus.
- First run: show value fast, ask for as little as possible up front, let people skip, and teach in context rather than with a long tour.
- Never lose in-progress input when navigating away: save a draft or warn.

## Mobile specifics
- Set the right input type and keyboard per field (email, tel, number, url) and `autocomplete` values so the OS can fill.
- The on-screen keyboard must not cover the focused field or the submit button; scroll it into view.
- Respect safe areas (notch, home indicator) and keep primary actions in thumb reach.
- No hover-only information; long-press and swipe gestures need a visible alternative.
- Support both orientations where it makes sense and slow connections (small payloads, skeletons).
