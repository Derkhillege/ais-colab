# Triage Labels

The skills speak in terms of five canonical triage roles. This file maps those roles to the actual strings used in this repo's issue tracker.

This repo tracks issues as local markdown files (see `issue-tracker.md`), so a "label" is written as a `Status:` line near the top of the issue file rather than applied through a tracker API:

```markdown
Status: needs-triage
```

| Label in mattpocock/skills | String in our `Status:` line | Meaning                                  |
| -------------------------- | ---------------------------- | ---------------------------------------- |
| `needs-triage`             | `needs-triage`               | Maintainer needs to evaluate this issue  |
| `needs-info`               | `needs-info`                 | Waiting on reporter for more information |
| `ready-for-agent`          | `ready-for-agent`            | Fully specified, ready for an AFK agent  |
| `ready-for-human`          | `ready-for-human`            | Requires human implementation            |
| `wontfix`                  | `wontfix`                    | Will not be actioned                     |

When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the corresponding string from this table.

Edit the middle column to match whatever vocabulary you actually use. If this repo later moves to a hosted tracker, these same five roles become real labels there.
