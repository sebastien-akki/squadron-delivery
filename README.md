# Delivery notes

Writes a project's delivery note for its client, from what the repository and Jira say about a period.

## Use

- In an agent's ⋯ menu, **Write the delivery note** puts the request in your message; add a period, an environment or a version if you want (`depuis le 1er novembre, en staging`), then send it.
- The agent uses the `delivery:notes` skill:
  - the period: the one you gave, else since the last tag, else since the last commit that mentions a delivery, else the last 14 days;
  - the commits of the main branch, the merged pull requests (`gh` on GitHub, or the tools of your forge's Squadron extension), and the Jira keys found in them, read with the Jira extension's `jira_ticket` tool when it is installed;
  - what the client never sees is left out (refactors, tests, CI);
  - the note: **New**, **Improved**, **Fixed** and **To test**, in plain words, in your language (French by default).
- It comes as an artifact to share and as Markdown in the reply, ready for an email.

You can also ask an agent for it in your own words: the skill is available to every agent once allowed.

## What it asks Squadron for

- **A skill**, `delivery:notes`, given to your agents once you allow it. No program, no network, no WebAssembly.
