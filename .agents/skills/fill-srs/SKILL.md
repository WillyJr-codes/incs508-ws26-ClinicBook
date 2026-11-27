---
name: fill-srs
description: Use when a ClinicBook team member asks to fill in, draft, write or update their part of M1/SRS.md, claim an SRS section or feature, or add themselves to the README team table.
---

# fill-srs

Draft one team member's part of `M1/SRS.md` and their own `README.md` team row, from an interview with that member, without changing any other member's text.

**Core principle:** the member decides, you write. Every policy, number and rule in the draft comes from the case (`docs/case-clinicbook.md`), from text already in the SRS, or from an answer the member gave in this session. Nothing else.

## Files

| File | Role |
|---|---|
| `M1/OWNERS.md` | Who owns what, and the rules for shared areas. Read it fully first. |
| `M1/SRS.md` | The SRS. Edit only the member's parts and the member's rows. |
| `docs/case-clinicbook.md` | The case. Source for the Case trace column. |
| `.agents/skills/humanizer/SKILL.md` | Rewrites every drafted passage (step 6). |
| `AI-USE.md` | One log row per session (step 9). |
| `M1/prototype/` | Read-only, may be absent. Source for the Prototype trace column. |

## Workflow

Do the steps in order. Steps 3 to 7 repeat for each part the member owns.

### 1. Sync and identify

1. Run `git config core.hooksPath`. If it does not print `.githooks`, run `git config core.hooksPath .githooks` so the repo's safety hooks are on. Never bypass them; if a hook blocks a commit or push, fix what it reports.
2. Run `git status --short`. If `M1/SRS.md`, `M1/OWNERS.md`, `README.md` or `AI-USE.md` has uncommitted changes, show them to the member and ask what to do before going on.
3. Run `git switch Itr1`, then `git pull --rebase origin Itr1`.
4. Read `git config user.email`, `git config user.name`, and `gh api user -q .login` if `gh` is installed. Match them against the Members table in `M1/OWNERS.md`.
5. Tell the member who you think they are and which parts they own, and ask them to confirm. If there is no match, ask for their GitHub username, full name and student ID, and add their Members row in step 2.

### 2. Claim (only if the member wants a part they don't own yet)

- Sync first (see "Syncing with the remote" below), then check that the Owner cell is still empty in the pulled file.
- Unclaimed part (Owner cell empty): put the member's username in the Owner cell, commit only `M1/OWNERS.md` with the message `OWNERS: <username> claims <parts>`, then push. Do this before drafting, so teammates see the claim.
- Part owned by someone else: do not claim it and do not write it. Tell the member who owns it and that the owner has to agree to hand it over in `M1/OWNERS.md` first.

### 3. Read before asking

Read the whole `M1/SRS.md`, the case, and the prototype if it exists. For the part you are about to draft, list:

- **Open decisions:** every policy or number the part needs that neither the case nor the SRS states. The case's "Deliberately Left Open" list is the main source of these. See the question bank below.
- **Conflicts:** places where another member's text contradicts what this part will say. These go to step 8 and are not fixed in their text.

### 4. Interview, one part at a time

Ask about the open decisions for this part in one message. Give a suggested answer for each question, drawn from the case or prototype and labelled as a suggestion, so the member can reply "ok" or change only what they disagree with. If the answers raise new open decisions, ask a follow-up round. Ask the member to set the priority (High/Medium/Low) of each requirement, or to accept the suggested priorities.

**The interview is never skipped.** When the member says "no time" or "don't ask me questions", put all their parts' questions into one message with suggested answers and say that one reply ("ok", or the changes) is enough. A decision the member did not confirm stays out of the draft; it becomes a TBD item instead (step 5).

### 5. Draft

Write the part in the section's existing heading structure. Requirements use this exact form, one testable behavior each:

```
- **R07:** The system shall send each patient a reminder 24 hours before a booked appointment. (High)
```

- **IDs:** take the next free R-number in the Requirements Overview table, after the latest pull. Never reuse or renumber another member's ID.
- **Overview row** for each requirement: `| R07 | <short paraphrase> | FR, NFR or C | F<n>, or the 5.x section number | <BG1–BG4, "Constraint: …" or "Scope: …" from the case> | <prototype screen, or — if none or no prototype> |`. Fill the first empty `| Rnn | | | | | |` row, or append a row if none is empty.
- **Glossary:** add a row `| Term | Definition | <username> |` for each new term the part uses. If a term already exists under another member, use theirs as written.
- **TBD:** add a row `| <next #> | <open decision> | <section> | <username> | Open |` for each open decision the member left unanswered.
- **Revision History:** add one row `| <next version> | <YYYY-MM-DD> | <username> | <parts drafted> |`.

### 6. Humanize

Read all of `.agents/skills/humanizer/SKILL.md` (every one of its 26 patterns, in chunks if needed) and apply it in embedded mode to every prose passage you drafted: descriptions, stimulus/response text and requirement sentences. Leave IDs, "The system shall", priorities, table structure and headings as they are.

### 7. Approve, then write

Show the member the humanized text for the part, plus every shared-area row you will add. That is the exact text that goes into the files: after approval, write it unchanged, with no further humanizer pass or rewording. Write only after the member approves. If they change anything, apply the change and show the text again.

After approval, sync again before you write (see "Syncing with the remote" below), because teammates may have pushed while the member read the draft. Then read `M1/SRS.md` again and check the pulled file:

- If someone took the R-numbers, TBD numbers or Revision History version you planned to use, move the member's items to the next free numbers. Keep the approved text otherwise unchanged and report the changed IDs (old → new) in step 10.
- If a teammate's new text now conflicts with the approved text (the same glossary term, a contradicting rule, an edit inside the member's part), stop before writing, show the member what changed and agree what to do.

Commit and push (steps 9 and 10) right after writing. Do not leave written text uncommitted while you do something else.

### 8. Other members' text

Never edit, reword, move or reformat another member's part or rows, even when the member asks or says the owner won't mind. For each conflict or error you found in someone else's part:

1. If `gh` is installed and logged in, run `gh issue create --title "SRS <section>: <problem>" --body "<what conflicts, quoting both texts, and the suggested fix>" --assignee <owner>`, then give the member the issue link.
2. Otherwise, give the member a short message they can send to the owner, with the same content.

### 9. Log the AI use

Add one row to the Log table in `AI-USE.md`, using its existing columns:

```
| <next #> | <YYYY-MM-DD> | <tool and model, e.g. Claude (Anthropic), Sonnet 5.5> | <member's full name> | Draft SRS <parts> with the fill-srs skill after interviewing the member | <parts, R-IDs and shared rows written> | Member answered the interview and approved the final text of <parts>; <any edits the member made> |
```

If `AI-USE.md` is missing, create it with this content, then add the row:

```
# AI Usage Log

AI agents were used to draft parts of M1/SRS.md through the fill-srs skill. Each use is logged below.

## Log

| # | Date | Tool | Team member | Purpose | Output produced | Verification and modifications |
|---|---|---|---|---|---|---|
```

If the file says that no AI was used for `M1/SRS.md`, replace that sentence with: `AI agents were used to draft parts of M1/SRS.md through the fill-srs skill. Each use is logged below.`

### 10. Commit and push

1. Run `git diff`. Every changed line must be inside the member's own parts or rows, the AI-USE row, or the member's README row. If any other line changed, undo that change before committing. Never convert line endings or reformat a file.
2. README: edit only the member's own Team row (`| Student ID | Full name | GitHub username |`). Fill the first empty row if they have none.
3. Commit `M1/SRS.md`, `AI-USE.md` and `README.md` (if changed) with the message `SRS: <parts> (<username>)`.
4. Run `git log --oneline origin/Itr1..HEAD` and check that only this session's commits are listed. Sync (see "Syncing with the remote" below), then run `git push origin Itr1`. If the push is rejected, sync and push again.
5. If the rebase conflicts and the conflict is only that someone took the same new R-number, renumber the member's own requirements to the next free numbers, update their Overview rows, and continue the rebase. For any other conflict, run `git rebase --abort` and tell the member what conflicted.
6. Finish by telling the member what was written, the commit hash, any requirement IDs that changed since they approved the text (old → new), and any issues or messages from step 8.

## Syncing with the remote

Teammates push to `Itr1` throughout the day, so a pull at the start of the session is not enough. Sync before every write to `M1/OWNERS.md`, `M1/SRS.md`, `README.md` or `AI-USE.md`, after any pause or resumed session, and before every push. The full rules are in `AGENTS.md` under "Always work on the latest remote".

1. Run `git fetch origin`, then `git status -sb`. The first line must read `## Itr1...origin/Itr1`. If it shows another branch or a detached HEAD, run `git switch Itr1`.
2. If it shows `[behind N]`, or before a push, run `git pull --rebase origin Itr1`.
3. If uncommitted files block the pull (for example a member's own edit to another file), run `git stash push <path>` for those files, pull, then `git stash pop`. Never discard them. If `git stash pop` conflicts, stop and show the member.
4. If the pull brought new commits, read the files you are about to edit again before editing them.
5. If `git fetch` or `git pull` fails, stop writing and tell the member. Do not write to a copy you cannot check against the remote.

## Question bank

| Part | Open decisions to ask about |
|---|---|
| Booking features (book, change, cancel) | appointment types and durations; booking horizon; cancellation and change cutoff; what happens after the cutoff; opening hours |
| Reminders | channels; timing; content (and what it must not reveal); languages |
| No-shows | what counts as a no-show; consequences; who records it |
| Reception and clinician views | who sees which fields; counseling confidentiality |
| MediDesk (2.1, 3.3) | which data flows which way, and when |
| 1.x Introduction | team conventions (priority scale, ID format); intended readers |
| 2.4 to 2.7 | supported devices and browsers; budget and deadline figures; assumptions about university login |
| 5.1 Performance | expected volumes and peak periods; response times |
| 5.3 Security | who may see which data; logging; session rules |
| 5.4 Quality | supported languages; accessibility; availability |

## Red flags

Stop and go back to step 4 if you are about to:

- write a number or rule (a time limit, a channel, a default, a law) that the member did not confirm and the case does not state
- skip the interview because the member is in a hurry
- edit a line outside the member's own parts or rows, including "just fixing" an obvious error
- write to the files before the member has seen and approved the final text

Stop and sync first (see "Syncing with the remote") if you are about to write to a file or push without a fetch since the member's last reply.
