# ux-fix

A Claude Code skill that audits a screen or flow for UX problems and fixes them: missing loading/empty/error/success states, bad error messages, form friction, button states, tap targets, accessibility, edge cases, UI copy, UI conventions, too many choices.

## Install

```bash
git clone https://github.com/nvictorinn/ux-fix ~/.claude/skills/ux-fix
```

Restart the session. Ask for a UX audit, or say "fix the UX of this screen".

## Usage

```
/ux-fix  (or /ux-fix all)      full audit of the current work
/ux-fix src/app/recipe/edit.tsx full audit of a file or flow
/ux-fix forms                  one topic
/ux-fix checkout errors, a11y  a target and several topics
/ux-fix forms short            condensed report, one line per finding
```

Reports are detailed by default (`long` says it explicitly). After a short report, ask for any finding by its number to get the full explanation.

Findings are ranked by severity and frequency. Every blocker is always shown, then the next findings up to 10, and the rest on request.

Topics: `states`, `loading`, `partial`, `errors`, `empty`, `success`, `forms`, `buttons`, `targets`, `conventions`, `choices`, `complexity`, `copy`, `a11y`, `edge`, `navigation`, `mobile`.

## Files

- `SKILL.md`: method and checklist.
- `references/rules.md`: the rules with numbers and examples (states, loading, errors, forms, buttons, accessibility, edge cases, navigation, mobile, UX laws).
- `references/copy.md`: UI copy rules and templates (buttons, errors, empty states, success, confirmations).
- `references/examples.md`: before/after code for the common fixes.

## License

MIT, see `LICENSE`. `references/copy.md` is adapted from the `pm-uxwriting` skills in [pm-claude-skills](https://github.com/mohitagw15856/pm-claude-skills) by Mohit Aggarwal (MIT).
