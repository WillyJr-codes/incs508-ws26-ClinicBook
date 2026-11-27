# Agent instructions

This repository holds the INCS508 (Requirements Engineering, Winter 2026, GIU) team project for the ClinicBook case: an SRS and a clickable prototype. Several team members work on the same files with different AI agents, so these rules apply to every agent in every session. The repo owner is GitHub user `WillyJr-codes`.

## Skills in this repo

| Skill | Canonical file | Use it when |
|---|---|---|
| fill-srs | `.agents/skills/fill-srs/SKILL.md` | A member asks to fill in, draft or update their part of `M1/SRS.md`, claim a section or feature, or add their README team row |
| humanizer | `.agents/skills/humanizer/SKILL.md` | Any prose you write for the SRS or the docs (fill-srs calls it for you) |
| impeccable | Your tool's own copy (see below) | Any UI, UX or design work. See "UI, UX and design work" |

If your agent does not load skills automatically, read the canonical file and follow it. The fill-srs and humanizer copies in `.claude/skills/` are stubs that point to the canonical files; edit only the canonical ones.

Impeccable is a third-party skill (https://impeccable.style, Apache 2.0). Each tool has its own build, because the builds differ in their internal paths:

| Tool | Impeccable skill folder | Impeccable subagents |
|---|---|---|
| Codex and any agent that reads `.agents/` | `.agents/skills/impeccable/` | `.agents/skills/impeccable/agents/*.toml` (inside the skill) |
| Claude Code | `.claude/skills/impeccable/` | `.claude/agents/impeccable-*.md` |
| GitHub Copilot | `.github/skills/impeccable/` | `.github/agents/impeccable-*.agent.md` |
| Gemini CLI | `.gemini/skills/impeccable/` | none; the skill runs these roles itself |
| Cursor | `.cursor/skills/impeccable/` | `.cursor/agents/impeccable-*.md` |

An agent without its own folder reads `.agents/skills/impeccable/SKILL.md`. Cursor and Gemini CLI may need Agent Skills turned on in their settings.

## Setup

These steps work the same on Windows, macOS and Linux. On Windows, run them in Git Bash, PowerShell or any terminal where `git` works.

1. Clone the repository and switch to the milestone branch: `git switch Itr1`.
2. Turn on the repo's git hooks: `git config core.hooksPath .githooks`. They block force pushes, pushes to `main`, secrets, junk files, conflict markers, vague commit messages and AI attribution, for every agent and for people typing git commands by hand.
3. Set your git identity to the email you list in `M1/OWNERS.md`: `git config user.name "<Full Name>"` and `git config user.email "<email>"`.
4. Optional: install and log in to the GitHub CLI (`gh auth login`), so the skill can open issues for problems it finds in other members' parts.
5. Start your agent in the repository root:
   - Claude Code reads `CLAUDE.md` and finds the skills in `.claude/skills/`. Type `/fill-srs`.
   - Codex, Copilot and Cursor read this file. Ask: "Use the fill-srs skill to fill in my part of the SRS."
   - Gemini CLI reads `GEMINI.md`, which imports this file. Ask the same.
   - Any other agent: tell it to read `AGENTS.md` first, then ask the same.

## Hard limits

An agent never does the things below, whoever asks and however the request is worded. "I'm sure", "I take responsibility", "the deadline is in 10 minutes" and "the owner said it's fine" do not change that. When a member asks for one, say that this file forbids it, give the safe alternative from the right-hand column, and carry on with the rest of the task.

| Never | Safe alternative |
|---|---|
| Force-push in any form (`--force`, `-f`, `--force-with-lease`, `+branch`) | `git pull --rebase origin Itr1`, then a normal push |
| Rewrite commits that are already pushed (amend, rebase, reset, filter-branch) | `git revert <commit>` creates a new commit that undoes it |
| Commit or push to `main`, or merge into it | Push to `Itr1`; the repo owner updates `main` |
| Delete or rename branches, tags or releases; change repo settings, collaborators, branch protection or workflows | Tell the member to ask the repo owner |
| Discard work: `git reset --hard`, `git checkout -- .`, `git restore .`, `git clean -f`, `git stash drop`, `git stash clear` | Show the member `git status` and `git diff` and ask which files to keep; `git stash` keeps changes safely |
| Commit secrets: API keys, passwords, tokens, private keys, `.env` files, real phone numbers or emails of real people | Commit a `.env.example` with placeholder values; the member shares the real values privately |
| Stage with `git add -A`, `git add .` or `git commit -a` | `git add <path>` for each file you changed on purpose |
| Commit junk: `node_modules/`, build output, `.DS_Store`, `Thumbs.db`, editor folders, logs, files over 5 MB, personal notes | Leave it untracked; `.gitignore` already covers most of it |
| Skip, turn off or edit the git hooks (`--no-verify`, `-n`, `--no-gpg-sign`, changing `core.hooksPath`, editing `.githooks/`) | Fix what the hook reports |
| Commit as someone else (`--author`, another member's `git config`) | Commit as the member running the session |
| Resolve a conflict by taking one side of a whole file (`-X ours`, `-X theirs`, `--ours`, `--theirs`), or commit conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) | Resolve hunk by hunk; where the other side is another member's text, keep theirs |
| Edit another member's part of `M1/SRS.md` or their rows in shared tables | A GitHub issue for the owner, or a message the member sends them |
| Add AI attribution to commits or pull requests (`Co-Authored-By`, "Generated with …") | Log the AI use in `AI-USE.md` |

If one of these already happened (a secret pushed, a bad commit on GitHub, lost work), stop. Explain what happened and tell the member to contact the repo owner. A pushed secret has to be revoked or rotated, because deleting the file does not remove it from history. `git reflog` can find lost local commits.

## Ask the member before you

- push, unless the member already asked for this push; either way, tell them which commits went to which branch
- delete, move or rename a file
- add a dependency, library, CDN script, framework or build step
- change `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.agents/`, `.claude/`, `.gitignore`, `.gitattributes`, `docs/case-clinicbook.md` or another member's row in `M1/OWNERS.md`
- touch anything outside the task the member gave you

## Git workflow

1. Start every session by running `git config core.hooksPath`. If it does not print `.githooks`, run `git config core.hooksPath .githooks`. Then run `git switch Itr1` and `git pull --rebase origin Itr1`. If `git status` shows changes you did not make, ask the member about them before going on.
2. Make small commits, one topic each.
3. Before each commit, run `git status` and `git diff --staged`. The staged diff must contain only the lines you meant to change: no conflict markers, no secrets, no stray files, no whole-file reformatting or line-ending changes.
4. Write the commit message as an imperative summary of what the commit actually changes, under 72 characters, e.g. `SRS: add F2 reminder requirements (alice-gh)`. "update", "fix" and "changes" alone are not messages. If a rebase or conflict resolution changed what a commit contains, reword the message to match before pushing.
5. Run `git pull --rebase origin Itr1` again, then `git push origin Itr1`. If the push is rejected, pull with rebase and push again.
6. If a rebase conflict touches only the member's own lines, resolve it. If it touches another member's text, keep their version or run `git rebase --abort`, then tell the member whom to talk to.

### Always work on the latest remote

Teammates push to `Itr1` all day. A pull at session start is not enough, because the remote can move while you interview the member or wait for approval. Edit only a copy that matches the latest `origin/Itr1`.

1. Before every edit to a tracked file, run `git fetch origin`, then `git status -sb`. The first line must read `## Itr1...origin/Itr1` with no `[behind N]`. If it shows `behind`, run `git pull --rebase origin Itr1` before you edit.
2. Pull again just before you write text the member has approved, and after any pause, resumed session or long wait. Do not write from a copy you pulled earlier.
3. After every pull that brought new commits, read the file again before you edit it. Take IDs (next free R-number, TBD number, Revision History version) from the file as it is now, not from an earlier read.
4. If uncommitted changes block the pull, run `git stash push <path>` for those files, pull, then `git stash pop`. Never discard them. If `git stash pop` conflicts, stop and show the member.
5. Commit and push as soon as a change is approved and written. Do not keep local commits unpushed, and do not start a second topic on top of an unpushed one.
6. Never edit on `main`, on a detached HEAD or on any other local branch. If `git status -sb` does not show `Itr1...origin/Itr1`, run `git switch Itr1` and pull first.
7. If `git fetch` or `git pull` fails (no network, no access), stop editing shared files and tell the member. Do not continue on a copy you cannot check against the remote.

### Good commits

- One logical change per commit. An SRS part, an OWNERS claim and a prototype fix are three commits.
- Commit only a working state: the SRS renders as valid Markdown, and the prototype opens with no console errors.
- When the reason for a change is not obvious from the summary line, add a short body after a blank line that says why.
- Before each push, run `git log --oneline origin/Itr1..HEAD` and check that exactly the commits you mean to send are listed.
- Keep diffs minimal: no unrelated whitespace, wrapping, reordering or line-ending changes. `.gitattributes` sets line endings; do not override it.
- Leave generated files, editor and agent folders (such as `.omc/`, `.serena/`, `.vscode/`) and personal notes untracked.

## Writing the SRS

- Use the fill-srs skill. It holds the interview, approval and logging steps.
- Policies, numbers and rules in the SRS come from `docs/case-clinicbook.md`, from text already in the SRS, or from the member's own answers. Ask the member about anything else.
- Requirements read "The system shall …", one testable behavior each, with a priority (High, Medium or Low).
- Run prose through the humanizer skill.
- Every session that writes SRS or documentation text adds a row to `AI-USE.md`.
- Apply the PDRE principles below to every requirement you write, and check your draft against the smell list before you show it to the member.

## PDRE: Principle-Driven Requirements Engineering

Source: Lecture 8 (Dr. John Zaki). It applies the code principles SOLID, KISS, DRY and YAGNI to requirements, so that the SRS does not carry immature business artifacts into design and testing. It holds at every scale: a whole section or one line of acceptance criteria.

| Principle | Rule for the SRS | Wrong → right |
|---|---|---|
| S: Single Responsibility | One requirement owns one business need and one owner. Split a requirement that covers several needs or user classes. | "Register, pay and get a timetable by email" → three requirements |
| O: Open/Closed | Extend an agreed requirement with a new numbered requirement. Do not rewrite it once the team has agreed on it. Log the change in Revision History. | Edit R-12 "credit card" → keep R-12, add R-12.1 "mobile wallet" |
| L: Liskov Substitution | A detailed requirement must not contradict its parent feature or goal. If it does, change the parent or move the child to another feature. | Feature "all patients book online", one requirement "front desk only, no limit" |
| I: Interface Segregation | Write requirements per user class. Do not force one class to review requirements it never uses. | One "dashboard for all users" → one per user class |
| D: Dependency Inversion | Point to business abstractions and to the interface sections (3.x), not to vendors, tables or other requirements' details. | "SMS through vendor X" → "notify through the patient's chosen channel (SRS 3.4)" |
| KISS | One reading only: clear, specific, testable. No "reasonable", "appropriate", "as needed", "etc." | "within a reasonable time" → "until 24 hours before the slot" (value from the case or the member) |
| DRY | State each rule once, as a business rule in 5.5, and say "Applies BR-x" elsewhere. Do not repeat a number or policy in several places. | Cutoff written in three requirements → one BR, three references |
| YAGNI | Write only what the case or a stakeholder asked for. Move other ideas to Appendix C or the backlog and mark them as not asked for. | Invented AI or multi-language feature → left out |

Smells to check before you show a draft:

| Smell | How to notice it | Fix with |
|---|---|---|
| Large Business | One requirement owns several needs or user classes | S + I |
| Feature Envy | A requirement quotes other requirements' rules | S + D |
| Cyclomatic Complexity | One requirement has many branches; nobody can list its test cases | KISS + S (decision table) |
| Shotgun Surgery | One rule change means edits in many places | DRY + D + O |
| Duplicate Business | The same need is stated twice in different words | DRY |
| Dead Business | An obsolete or never-asked requirement is still there | YAGNI; remove it and log it in Revision History |

Rules for agents:

- Run the principle and smell check on your own draft and show the member the result with the text.
- Fix a smell in the member's own part. For another member's part, do not edit; open an issue or give the member a message, as in the hard limits.
- A policy or number that the case does not give is a TBD in Appendix C, not an invented value (KISS and YAGNI).
- Removing a requirement is allowed only for the member's own requirements, with a Revision History row.

## Prototype code (`M1/prototype/`)

`M1/prototype/` is read-only during SRS work. For prototype tasks:

- Invoke the impeccable skill for every change to a screen, as described in "UI, UX and design work".
- Keep it plain HTML, CSS and JavaScript that runs by opening `index.html`. Adding a framework, package manager, build step or new CDN script needs the member's approval and the team's agreement.
- Read the surrounding code first and match its style, naming and structure. Change only what the task needs: no rewriting whole files, mass reformatting or renaming things the task does not mention.
- Settings belong in `config.js`, UI text in `i18n.js` (English and German both), and requirement links in `traceability.js`. Requirement IDs in `traceability.js` must match `M1/SRS.md`.
- Leave no `console.log` debugging, commented-out code or dead code.
- Before committing, open `index.html` in a browser, walk through the steps of the demo script in `M1/prototype/README.md` that cover the screens you changed, check that the browser console shows no errors, and switch between English and German if you changed text. Tell the member exactly what you tested.
- Log AI-written code in `AI-USE.md` like any other AI use.

### Coding practice

The PDRE principles come from code. Apply them to the prototype in their original form:

- KISS: write the simplest code that does the job and that a teammate can read without you. Prefer plain functions and the browser's built-in features over clever tricks.
- DRY: keep one source of truth. Reuse an existing helper, setting or text key instead of copying it; put a repeated value in `config.js`.
- YAGNI: build only what the task and the SRS ask for. No speculative options, hooks or abstractions.
- Single responsibility: one function does one thing. Split a function that handles several screens or concerns.
- Names say what a thing is or does (`bookSlot`, `freeSlots`), not how it is built (`data2`, `handleStuff`). Use `const` by default, `let` only when a value changes, never `var` or undeclared globals.
- Handle errors and empty states on purpose: no slots, a slot taken a moment ago, missing settings. Show the user a clear message; never fail silently.
- Insert user or sample text with `textContent`, not `innerHTML`, so text cannot run as code.
- Keep it accessible: every input has a label, every action works with the keyboard, focus is visible, and text has enough contrast.
- Comment why, not what. Leave no TODO notes, stubs or half-built branches; finish them or tell the member they are open.
- Each requirement a screen implements is listed in `traceability.js`, so code and SRS stay in step.

## UI, UX and design work: Impeccable

Every task that creates or changes what a user sees or does goes through the impeccable skill: prototype screens, layout, styling, colors, fonts, motion, UI text, forms, empty and error states, accessibility, and design reviews. Invoke it before you edit and follow its playbook. Do not design or restyle UI from your own defaults. Impeccable is for interfaces only; SRS prose uses fill-srs and humanizer.

How to invoke: `/impeccable <command> <target>` in Claude Code, Copilot, Gemini CLI and Cursor; `$impeccable <command> <target>` in Codex. `/impeccable` alone lists the commands. If your agent has no slash commands, read its `SKILL.md` and follow it with the command name.

Pick the command that matches the task:

| Task | Command |
|---|---|
| Set up project context, once for the team | `init` writes `PRODUCT.md`; `document` writes `DESIGN.md` |
| Plan a screen or flow before writing code | `shape` |
| Build a new screen | `/impeccable` with a plain build request |
| Review a design | `critique` (scored UX review) and `audit` (accessibility, performance, theming, responsive behavior; P0 to P3) |
| Steer the look | `bolder`, `quieter`, `distill`, `colorize`, `typeset`, `layout`, `animate`, `delight`, `overdrive` |
| Finish a screen | `polish` (final pass), `harden` (errors, i18n, overflow, edge cases), `clarify` (unclear UI text), `adapt` (screen sizes), `optimize` (performance), `onboard` (first run and empty states) |
| Pull repeated patterns into the design system | `extract` |

Rules for agents:

- Before you edit UI, check that `PRODUCT.md` and `DESIGN.md` exist at the repo root. If they are missing, tell the member and suggest `init` and `document`; the team agrees on them once, because every later command reads them.
- Impeccable works inside this repo's limits: plain HTML, CSS and JavaScript, no framework, build step or new CDN script (see "Prototype code"), English and German text in `i18n.js`, and the SRS rules for what each user class may see. If a playbook suggests something these rules forbid, follow the rules and tell the member.
- Before you report UI work as done, run `audit` and `critique` on every screen you changed. Fix P0 and P1 findings, and list any you leave open.
- The design hook is not installed, so nothing checks UI edits automatically. The checks above replace it.
- Do not edit the impeccable subagent files. They are upstream files, like the skill folders.
- The first run downloads the impeccable engine into `~/.impeccable/bin/`, checked against `scripts/engine-checksums.txt`. Set `DO_NOT_TRACK=1` to switch off its anonymous usage telemetry.
- Commit `PRODUCT.md`, `DESIGN.md` and the shared files under `.impeccable/` (`config.json`, `design.json`, `surfaces/*.md`, `critique/ignore.md`). Leave the rest of `.impeccable/` (screenshots, caches, reports) untracked.
- Do not edit the impeccable skill folders. They are upstream files. To update them, ask the member, then copy the new build from https://github.com/pbakaus/impeccable into all five skill folders and the three subagent folders (`.claude/agents/`, `.cursor/agents/`, `.github/agents/`) in one commit that names the upstream version.

### Impeccable subagents

Impeccable hands some steps to subagents: helpers with a narrow job that run in their own context and report back. The skill's playbooks start them at the right step, so you normally do not call them yourself.

| Subagent | What it does | Started during |
|---|---|---|
| `impeccable-finish-reviewer` | Fresh-eyes review of a finished build against its direction and quality bar. Returns an ordered list of fixes and edits nothing. | The end of a new build |
| `impeccable-motion-reviewer` | Reviews captured animation for timing, continuity and reduced-motion behavior. Returns fixes with evidence. | `animate` |
| `impeccable-documenter` | Writes `DESIGN.md` from the finished screens, not from intentions. | The end of a new build |
| `impeccable-asset-producer` | Turns approved mock references into clean reusable images without redesigning them. | A build from an approved comp |
| `impeccable-manual-edit-applier` | Applies text edits picked in live mode to the source files. | `live` |

How each tool runs them:

- Claude Code: the skill starts them itself. You can also ask for one by name, for example "use the impeccable-finish-reviewer subagent on the booking screen". `/agents` lists them.
- Codex: the skill finds them inside its own folder, named with underscores (`impeccable_finish_reviewer`). Codex runs subagents only when the member allows sub-agents or parallel work. If that is off, the skill asks once and stops; tell the member and let them decide.
- Cursor: the agent may delegate to them on its own, or the member types `/impeccable-finish-reviewer` (or another name).
- GitHub Copilot: say "Use the impeccable-finish-reviewer agent" (or another name), or let the skill request it.
- Gemini CLI and agents with no subagent support: the skill does the same work inline from `reference/degraded/`. The result is the same; it uses more of the main context.

Rules:

- Reviewer subagents (`finish-reviewer`, `motion-reviewer`) only report. The main agent applies their fixes, inside this repo's limits, and tells the member which fixes it applied or skipped.
- Pass a subagent only the files its job needs: the screens you changed, `PRODUCT.md`, `DESIGN.md` and the captures. Never pass `.env` files, credentials or personal data.
- The `documenter` writes `DESIGN.md`, which the whole team shares. Show the member the result before committing it.

## Before you say you are done

Tell the member which files changed, the commit hash and branch, what you tested, and what you did not do or could not check. Say plainly when something failed.

## Layout

| Path | Contents |
|---|---|
| `M1/SRS.md` | Software Requirements Specification (IEEE 830 / Week 7 template) |
| `M1/OWNERS.md` | Who owns which SRS part, and the shared-area rules |
| `docs/case-clinicbook.md` | The ClinicBook case, transcribed verbatim |
| `docs/TEAM-GUIDE.md` | How team members use the skill |
| `AI-USE.md` | AI usage log (may be missing until the first logged use) |
| `.githooks/` | Git hooks that enforce the hard limits (POSIX `sh`, run by git on every platform) |
| `.agents/skills/` | Canonical skills, readable by any agent, and the generic impeccable build |
| `.claude/skills/impeccable/`, `.github/skills/impeccable/`, `.gemini/skills/impeccable/`, `.cursor/skills/impeccable/` | Impeccable builds for Claude Code, Copilot, Gemini CLI and Cursor (upstream files, do not edit) |
| `.claude/agents/`, `.cursor/agents/`, `.github/agents/` | Impeccable subagents for Claude Code, Cursor and Copilot (upstream files, do not edit) |
| `PRODUCT.md`, `DESIGN.md` | Product and design context that impeccable reads (created by `/impeccable init` and `document`) |
| `CLAUDE.md`, `GEMINI.md`, `.github/copilot-instructions.md` | Pointers to this file for agents that look for their own file name |
| `.claude/settings.json` | Extra blocks for Claude Code users (deny rules, no attribution) |
