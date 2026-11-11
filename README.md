# nv-skills

A Claude Code plugin with my skills. Each skill runs as `/nv-skills:<skill>`.

## Install

```
/plugin marketplace add nvictorinn/nv-skills
/plugin install nv-skills@nv-skills
```

Restart the session. Update later with `/plugin marketplace update nv-skills`.

If you installed ux-fix before by cloning it into `~/.claude/skills/ux-fix`, delete that folder, or the skill loads twice.

## Skills

### ux-fix

Audits a screen, flow or whole app for UX problems and fixes them: missing loading/empty/error/success states, bad error messages, form friction, button states, tap targets, accessibility, edge cases, UI copy, UI conventions, too many choices. Ask for a UX audit, say "fix the UX of this screen", or use the command:

```
/nv-skills:ux-fix  (or ... all)            full audit of the current work
/nv-skills:ux-fix src/app/recipe/edit.tsx  full audit of a file or flow
/nv-skills:ux-fix forms                    one topic
/nv-skills:ux-fix checkout errors, a11y    a target and several topics
/nv-skills:ux-fix forms short              condensed report, one line per finding
```

Reports are detailed by default (`long` says it explicitly). After a short report, ask for any finding by its number to get the full explanation.

Every report says which screens were covered and which were not. A whole-app audit first lists every screen, sheet, menu and permission screen, so none is skipped silently. It then checks groups of screens in parallel subagents, plus one for issues that cross screens (midnight, offline, data changed elsewhere), so one run replaces several passes. It costs more tokens than a single-screen audit.

Findings are ranked by severity and frequency. Every blocker is always shown, then the next findings up to 10, and the rest on request.

Topics: `states`, `loading`, `partial`, `errors`, `empty`, `success`, `forms`, `buttons`, `targets`, `conventions`, `choices`, `complexity`, `copy`, `a11y`, `edge`, `navigation`, `mobile`.

## Files

- `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`: plugin and marketplace manifests.
- `skills/ux-fix/SKILL.md`: method and checklist.
- `skills/ux-fix/references/rules.md`: the rules with numbers and examples (states, loading, errors, forms, buttons, accessibility, edge cases, navigation, mobile, UX laws).
- `skills/ux-fix/references/copy.md`: UI copy rules and templates (buttons, errors, empty states, success, confirmations).
- `skills/ux-fix/references/examples.md`: before/after code for the common fixes.

A new skill goes in `skills/<name>/SKILL.md`.

## License

MIT, see `LICENSE`. `skills/ux-fix/references/copy.md` is adapted from the `pm-uxwriting` skills in [pm-claude-skills](https://github.com/mohitagw15856/pm-claude-skills) by Mohit Aggarwal (MIT).
