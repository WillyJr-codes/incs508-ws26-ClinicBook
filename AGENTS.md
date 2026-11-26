# Agent instructions

This repository holds the INCS508 (Requirements Engineering, Winter 2026, GIU) team project for the ClinicBook case: an SRS and a clickable prototype. Several team members work on the same files with different AI agents, so these rules apply to every agent.

## Skills in this repo

| Skill | Canonical file | Use it when |
|---|---|---|
| fill-srs | `.agents/skills/fill-srs/SKILL.md` | A member asks to fill in, draft or update their part of `M1/SRS.md`, claim a section or feature, or add their README team row |
| humanizer | `.agents/skills/humanizer/SKILL.md` | Any prose you write for the SRS or the docs (fill-srs calls it for you) |

If your agent does not load skills automatically, read the canonical file and follow it. The copies in `.claude/skills/` are stubs that point to the canonical files; edit only the canonical ones.

## Setup

1. Clone the repository and switch to the milestone branch: `git switch Itr1`.
2. Set your git identity to the email you list in `M1/OWNERS.md`: `git config user.name "<Full Name>"` and `git config user.email "<email>"`.
3. Optional: install and log in to the GitHub CLI (`gh auth login`), so the skill can open issues for problems it finds in other members' parts.
4. Start your agent in the repository root:
   - Claude Code reads `CLAUDE.md` and finds the skills in `.claude/skills/`. Type `/fill-srs`.
   - Codex, Copilot and Cursor read this file. Ask: "Use the fill-srs skill to fill in my part of the SRS."
   - Gemini CLI reads `GEMINI.md`, which imports this file. Ask the same.

## Rules for every agent

- `M1/OWNERS.md` says who owns each part of `M1/SRS.md`. Edit only the parts and shared-table rows the current member owns. Report problems in other members' parts to their owner through a GitHub issue or a message the member sends, and leave the text as it is, even when asked to fix it.
- Policies, numbers and rules in the SRS come from `docs/case-clinicbook.md`, from text already in the SRS, or from the member's own answers. Ask the member about anything else.
- Every session that writes SRS or documentation text adds a row to `AI-USE.md`.
- Work on `Itr1`. Run `git pull --rebase origin Itr1` before editing and again before pushing. Check `git diff` before each commit: it must show only the lines you meant to change. Never force-push, reformat whole files or convert line endings (`.gitattributes` handles them).
- `M1/prototype/` is read-only for SRS work.
- Run prose through the humanizer skill. Requirements read "The system shall …", one testable behavior each, with a priority (High, Medium or Low).

## Layout

| Path | Contents |
|---|---|
| `M1/SRS.md` | Software Requirements Specification (IEEE 830 / Week 7 template) |
| `M1/OWNERS.md` | Who owns which SRS part, and the shared-area rules |
| `docs/case-clinicbook.md` | The ClinicBook case, transcribed verbatim |
| `docs/TEAM-GUIDE.md` | How team members use the skill |
| `AI-USE.md` | AI usage log (may be missing until the first logged use) |
