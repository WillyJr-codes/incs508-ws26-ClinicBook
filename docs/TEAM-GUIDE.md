# Team guide: writing your part of the SRS

Each of us owns some parts of `M1/SRS.md`. You can write your parts yourself or have an AI agent draft them with the `fill-srs` skill. The skill interviews you, drafts only your parts, shows you the text, writes it after you approve, logs the AI use and pushes. It does not change anyone else's text.

## Requirements

| What | Needed? | Notes |
|---|---|---|
| Git | Yes | Windows, macOS or Linux. On Windows, install Git for Windows; it includes the `sh` that runs the repo's git hooks. |
| Write access to the repo | Yes | Ask the repo owner to add your GitHub account as a collaborator. Without it, the push at the end of a session fails. |
| An AI agent you are signed in to | Yes | Any agent that reads `AGENTS.md`, such as Claude Code, Codex, Copilot, Cursor or Gemini CLI. |
| GitHub CLI (`gh`) | No | Lets the skill open issues for problems in other members' parts. Without it, you get a message to send the owner. To set it up, install it from https://cli.github.com and run `gh auth login`. |

You don't need Python, Node or a separate humanizer install. The humanizer skill is already in the repo.

## Before your first session

1. Clone the repo and run `git switch Itr1`.
2. Turn on the safety hooks: `git config core.hooksPath .githooks`. Your agent also does this at the start of each session, but do it yourself too, so the hooks protect the git commands you type by hand.
3. Set your git identity: `git config user.name "Your Name"` and `git config user.email "you@example.com"`.
4. Open `M1/OWNERS.md` and add your row to the Members table (GitHub username, full name, git email). The skill can also do this for you.
5. Agree with the team who takes which feature (F1 to F5) and which sections. Free parts have an empty Owner cell. The skill can claim free parts for you.
6. Optional: set up the GitHub CLI (see Requirements).

## What the hooks block

| Hook | Blocks |
|---|---|
| `pre-commit` | `.env` files, private keys, API keys and tokens, `node_modules/`, build output, OS junk files, files over 5 MB, conflict markers |
| `commit-msg` | Vague messages ("update", "fix", anything under 10 characters) and AI attribution lines |
| `pre-push` | Pushes to `main`, force pushes (anything that would drop commits from GitHub), deleting branches |

When a hook blocks you, it prints the reason and what to do instead. Do the fix it suggests; don't work around it.

## For the repo owner

The `pre-push` hook also stops you from pushing to `main` from your machine. To update `main`, open a pull request from `Itr1` to `main` on GitHub and merge it in the browser. For a protection that no one can skip, turn on branch protection on GitHub (Settings, then Branches, then add a rule for `main` and `Itr1`): block force pushes and deletions, and for `main` require a pull request.

## Running it

Open your agent in the repo root and ask for your part:

| Agent | What to type |
|---|---|
| Claude Code | `/fill-srs` |
| Any other agent (Codex, Copilot, Cursor, Gemini CLI, …) | "Use the fill-srs skill to fill in my part of the SRS." If it doesn't know the skill, add "Read AGENTS.md first." |

The session goes like this:

1. The agent pulls the latest `Itr1`, works out who you are from your git email, and asks you to confirm which parts you own.
2. For each part, it asks about the decisions the case leaves open (cancellation cutoff, reminder channels, who sees what, and so on). Each question comes with a suggested answer, so "ok" is a valid reply. Change whatever you disagree with.
3. It drafts the part, runs it through the humanizer skill, and shows you the final text with the Overview, Glossary, TBD and Revision History rows it will add.
4. You approve or ask for changes. Nothing is written before you approve.
5. It adds a row to `AI-USE.md`, checks that the diff touches only your lines, commits as `SRS: <parts> (<username>)` and pushes.

Short on time? Say so. The agent puts all its questions in one message and one reply is enough. It still asks, because the decisions in your part have to be yours.

## What the skill will not do

- Edit another member's section, requirement, glossary term or table row, even when you ask. It opens a GitHub issue for the owner or writes you a message to send them.
- Claim a part someone else already owns.
- Invent policies or numbers you did not confirm. Unanswered decisions go into Appendix C (To Be Determined) under your name.
- Touch `M1/prototype/`.

## Rules for shared tables

| Area | Rule |
|---|---|
| Requirements Overview | Your rows are the ones whose Feature is a feature or 5.x section you own. New requirements take the next free R-number. |
| Glossary | Add your own terms. If a term already exists, use it as written; ask its author for a change. |
| To Be Determined list | Add and close only your own items. |
| Revision History | One new row per change, by you. |
| README Team table | Your own row only. |

## Before submission

- Read your parts once more in the final `M1/SRS.md`.
- Check your rows in `AI-USE.md` and add anything you changed or checked yourself to "Verification and modifications".
- Close or keep your TBD items on purpose.
