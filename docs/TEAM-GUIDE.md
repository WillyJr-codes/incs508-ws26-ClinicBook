# Team guide: writing your part of the SRS

Each of us owns some parts of `M1/SRS.md`. You can write your parts yourself or have an AI agent draft them with the `fill-srs` skill. The skill interviews you, drafts only your parts, shows you the text, writes it after you approve, logs the AI use and pushes. It does not change anyone else's text.

## Before your first session

1. Clone the repo and run `git switch Itr1`.
2. Set your git identity: `git config user.name "Your Name"` and `git config user.email "you@example.com"`.
3. Open `M1/OWNERS.md` and add your row to the Members table (GitHub username, full name, git email). The skill can also do this for you.
4. Agree with the team who takes which feature (F1 to F5) and which sections. Free parts have an empty Owner cell. The skill can claim free parts for you.
5. Optional: install the GitHub CLI and run `gh auth login`. Then the skill opens issues for problems it finds in other people's parts instead of handing you a message to send.

## Running it

Open your agent in the repo root and ask for your part:

| Agent | What to type |
|---|---|
| Claude Code | `/fill-srs` |
| Codex, Copilot, Cursor, Gemini CLI | "Use the fill-srs skill to fill in my part of the SRS." |

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
