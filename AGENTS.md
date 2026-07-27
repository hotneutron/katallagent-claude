Working rules. Keep them terse.

## Authority

- Local behavior: this file.
- Dirty working trees are evidence of local state, not committed truth.
- Specificity is not authority; verify speculative claims before building on
  them.

## Work Rules

Before proposing: name the source artifact and its authority.

Before implementing novel mechanism, protocol, or market-design surface: cite
or produce an existing-art study, or explicitly mark the study-gate warning.

After adding an instrument, axis, report, check, or simulator surface, exercise
the new observability before moving on, or explicitly record why it is
deferred.

## Repo Style

- Keep root docs focused on this repo.
- `katallagent/` is a symlink to the shared implementation repo
  `../katallagent`; both lanes read and modify it directly; Parallax rules do
  not apply there. Studies live under `katallagent/lab/`; the implementation
  lands outside `lab/`.
- Name docs `{YYMMDD}-{HHMM}-{type}-{topics}.md`; get timestamp from `date`.
- Doc types: `findings`, `reaction`, `reflection`, `cross-check`, `brainstorm`,
  `study`, `proposal`, `plan`, `schema_plan` (authority map: `cross-team.json` warrant).
- Any amendment to an existing artifact must add/update timestamped `## Revision History`.
- Prefer local adoption notes over copied partner history.
- No `Co-Authored-By` trailer.
