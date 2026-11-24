---
name: notes
description: Writes the delivery note of a project for its client, in plain words, from the commits, merged pull requests and Jira tickets of a period, as a page to share and a text to paste in an email. Use when the user asks for a delivery note, release notes, a changelog for the client, or "what we delivered".
---

# Delivery note for the client

Arguments: $ARGUMENTS (optional: the period, the environment, the version, the audience).

The note is for the client, not for developers: what they can do now, what works better, what was fixed, and what to test. Write it in the language the user writes in, French by default, with « » and a non-breaking space before `:` `;` `?` `!` in French.

## 1. The period

Take it from the arguments when given (`depuis le 2026-11-01`, `v2.3.0`, `staging`). Otherwise, in this order:

1. since the last tag on the main branch: `git describe --tags --abbrev=0 origin/HEAD`;
2. else since the last commit whose message mentions a delivery (`git log --grep -i -E "livraison|release|mise en prod|MEP"`);
3. else the last 14 days.

Say which period you took in the note's header, and ask only if the repository gives nothing usable.

## 2. Collect

- Commits of the main branch in the period: `git fetch -q` then `git log --no-merges --format='%h %ad %s' --date=short <from>..origin/HEAD`.
- Merged pull requests when a forge CLI is there: `gh pr list --state merged --search "merged:>=<date>" --json number,title,body,mergedAt` for GitHub; for Bitbucket or GitLab, use the tools of their Squadron extension when the session has them, else skip.
- Jira keys (`ABC-123`) found in commit messages, branch names and PR titles. When the session has a `jira_ticket` tool (the Jira extension), read each ticket's title and type; otherwise keep the key with the commit's wording.

## 3. Sort and write

- Drop what the client never sees: refactors, tests, CI, dependency bumps without effect, typos in code, merge commits. Keep a security update as one line ("Mises à jour de sécurité de la plateforme").
- Group by **Nouveautés**, **Améliorations**, **Corrections** (English: New, Improved, Fixed). One line per change, starting with what the user sees or does, then the ticket key in parentheses. Merge commits about the same change into one line.
- Add **À tester** (To test): 2 to 6 concrete checks on what changed, written as steps the client can follow.
- Header: project name, period, environment (staging or production when known), date of the note.
- No commit hashes, file names, class names or internal jargon. No promise about what is not delivered.

## 4. Deliver

- Make it an artifact the user can share: a folder of its own in `$SQUADRON_ARTIFACTS` (Squadron) holding `index.html`, a clean single page with the header, the sections and the checks, readable on a phone, with light and dark colors. When `$SQUADRON_ARTIFACTS` is not set, write `delivery-note-<date>.html` in the repository's root and say where.
- Then give the same note in your reply inside a ```markdown block, ready to paste in an email or a ticket.
- End with one line on what you could not check (for example tickets you could not read).
