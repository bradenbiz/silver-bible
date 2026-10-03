# Issue tracker: GitHub

Issues, specifications, and Wayfinder maps live in [bradenbiz/silver-bible](https://github.com/bradenbiz/silver-bible/issues). GitHub is the canonical tracker; `.scratch/` may hold temporary drafts but is not the tracker.

Use the authenticated `gh` CLI. The examples explicitly name the repository so another checkout or harness uses the same tracker.

## Conventions

- Create an issue: `gh issue create --repo bradenbiz/silver-bible --title "..." --body-file /path/to/body.md`.
- Read its details: `gh issue view NUMBER --repo bradenbiz/silver-bible --json number,title,body,labels,assignees,state,url`.
- Read its discussion: `gh issue view NUMBER --repo bradenbiz/silver-bible --comments`.
- List issues: `gh issue list --repo bradenbiz/silver-bible --state open --limit 100 --json number,title,labels,assignees,url`; add label filters as needed. Use paginated API queries when the result could exceed the limit.
- Edit an issue body: `gh issue edit NUMBER --repo bradenbiz/silver-bible --body-file /path/to/body.md`.
- Comment: `gh issue comment NUMBER --repo bradenbiz/silver-bible --body-file /path/to/comment.md`.
- Apply or remove a label: `gh issue edit NUMBER --repo bradenbiz/silver-bible --add-label "..."` or `--remove-label "..."`.
- Close a resolved issue: `gh issue close NUMBER --repo bradenbiz/silver-bible --reason completed`.

Replace uppercase placeholders with actual values. Write multiline bodies and comments to files and pass `--body-file`; preserve Markdown and literal shell characters.

When a skill says **publish to the issue tracker**, create a GitHub issue. When it says **fetch the relevant ticket**, read the issue details and comments. Refer to issues by linked titles in user-facing output.

## Pull requests as a triage surface

**PRs as a request surface: no.** Pull requests are code review artifacts. GitHub shares issue and pull request numbers; check the resource type when a supplied reference is ambiguous.

## Wayfinding operations

### Map and decision tickets

A map is one issue labelled `wayfinder:map`. Its body contains **Destination**, **Notes**, **Decisions so far**, **Not yet specified**, and **Out of scope**, following the Wayfinder skill. Confirm the destination through discussion before creating a map.

Each decision ticket is a child issue with a **Question** section and one type label:

| Label | Purpose |
| --- | --- |
| `wayfinder:map` | The map and index of decisions |
| `wayfinder:grilling` | A decision resolved through discussion with the owner |
| `wayfinder:research` | Research that informs a decision |
| `wayfinder:prototype` | A prototype needed to resolve a design decision |
| `wayfinder:task` | Prerequisite work that unblocks a decision |

These labels already exist in the repository. Wayfinder plans by default: decision tickets resolve questions, and task tickets unblock decisions rather than implement product features. Follow the skill's session limits and human-in-the-loop requirements.

### Native parent and blocking relationships

Create the issues first, then link them using GitHub's native sub-issues and dependencies.

Get an issue's numeric database ID:

```sh
gh api repos/bradenbiz/silver-bible/issues/NUMBER --jq .id
```

Attach a child to its map:

```sh
gh api --method POST repos/bradenbiz/silver-bible/issues/MAP_NUMBER/sub_issues \
  -F sub_issue_id=CHILD_DATABASE_ID
```

Record that a ticket is blocked by another issue:

```sh
gh api --method POST repos/bradenbiz/silver-bible/issues/TICKET_NUMBER/dependencies/blocked_by \
  -F issue_id=BLOCKER_DATABASE_ID
```

The URL uses an issue **number**. The request body uses a numeric **database ID**, not the issue number or GraphQL `node_id`.

### Find and claim the frontier

List the map's children in their returned order, keeping open, unassigned tickets:

```sh
gh api --paginate repos/bradenbiz/silver-bible/issues/MAP_NUMBER/sub_issues \
  --jq '.[] | select(.state == "open" and (.assignees | length == 0)) | {number, title, html_url}'
```

For each candidate, check for open blockers:

```sh
gh api --paginate repos/bradenbiz/silver-bible/issues/TICKET_NUMBER/dependencies/blocked_by \
  --jq '.[] | select(.state == "open") | {number, title, html_url}'
```

The frontier is the open, unassigned children with no open blockers. Take the first frontier ticket unless the user names one. Recheck its state and assignees immediately before claiming it; another session may have claimed it. The first write when working a ticket is its assignment:

```sh
gh issue edit TICKET_NUMBER --repo bradenbiz/silver-bible --add-assignee @me
```

Assignment is a coordination signal, not an atomic lock. Skip a ticket already claimed by another session, including another session using the same account.

### Resolve and update the map

Post the answer as a resolution comment, close the ticket, and append one linked title plus a one-line gist to **Decisions so far**. Keep the detailed decision in its ticket. Fetch the latest map body before editing it to preserve changes from other sessions.

Create newly specified tickets, wire their blocking relationships, and remove the corresponding text from **Not yet specified**. Follow Wayfinder's rules for closing work that falls outside the destination.

### Compatibility

Use native relationships whenever available. If a confirmed tracker limitation prevents sub-issues, use a task list on the map plus `Part of #MAP_NUMBER` on each child. If native dependencies are unavailable, use `Blocked by: #NUMBER` lines and check each blocker's state. Authentication errors or transient failures do not establish that a feature is unavailable.
