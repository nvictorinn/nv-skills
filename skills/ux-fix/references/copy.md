# UI copy

Words are part of the interface. Write for the moment, the user's goal and the stakes. Give 2 options when wording is a judgment call.

## Principles
- Lead with a verb on actions and say the outcome: "Save changes", "Send invoice", "Delete 3 files". Avoid bare "OK", "Submit", "Yes", "Click here".
- Say what happens next, especially when it is irreversible, costs money or notifies others.
- Short, scannable, plain words. No jargon, no internal terms, no codes.
- Blame-free: describe what to do, not what the user did wrong. "Enter an email like name@example.com", not "Invalid input".
- One name per concept across the product. Reuse existing vocabulary, do not invent terms.
- Tone matches stakes: light is fine for a cleared inbox, never for a failed payment or data loss.
- Clarity beats cleverness at decision points (buttons, confirmations, errors).
- Respect length limits and the existing voice.
- Write in the product's language; the examples here are in English. Handle plurals properly and leave room for longer translations.

## Error messages
Structure: what happened (plain words) + why or what to check (only if it helps) + next step (a button or clear instruction).
- Bad: "Error 500", "Invalid input", "Something went wrong".
- Good: "Your file wasn't uploaded: it's over the 25 MB limit. Compress it or pick a smaller one." [Choose another file]
- Sign-in: "Incorrect email or password." Never say which one, or whether the account exists.

Per surface:
- Inline field: short, next to the field, says how to fix. "Password needs at least 8 characters."
- Toast: only for minor or self-healing issues. "Connection lost. Retrying..."
- Modal or full page: what is blocked, why, and the way forward (retry, request access, contact support with a reference).

Developers: log the technical detail (stack, status code, request id); show the human message and, if useful, a short support reference.

## Empty states
Always headline + one supporting line + one primary action. Choose by cause:
- First use: the value and the first step. "No projects yet. Create one to start tracking your work." [Create project]
- Cleared or done: positive. "You're all caught up. Nothing needs your attention."
- No results: "Nothing matches 'oak desk'." Offer a way on: clear filters, broaden, check spelling, create it.
- No access or error: calm, what is wrong, next step (retry, request access, contact the admin).

Never ship a bare "No data". Never make "all done" look like an error.

## Success messages
Say what was done, in past tense, and what is next if relevant. "Invoice sent to Dana. View invoice." Keep it as small as the action: an inline check or toast for minor actions, a page for milestones.

## Forms
- Labels: noun or short question, always visible. Helper text for format and why you ask. Mark required (or mark optional if most are required).
- Validation text appears next to the field, in plain words, and says the fix.
- Primary button names the result ("Create account"). Secondary actions are visually quieter.

## Confirmations
Prefer undo. If a dialog is needed: title states the action and object ("Delete 'Q3 report'?"), body states the consequence ("This removes it for everyone and cannot be undone."), buttons are verbs ("Delete report" / "Keep it"), the destructive one is not the default.

## Loading and progress text
Specific and changing beats generic: "Checking your file", "Uploading 3 of 8 files". Never "Please wait" alone.

## Consistency notes
After writing, list the terms and patterns to reuse (button names, tense, capitalization, date and number format) so the product speaks in one voice.
