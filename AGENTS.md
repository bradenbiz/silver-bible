# Silver Bible

Silver Bible is in the planning stage. The product direction and technology stack are still to be decided with the project owner.

## Agent skills

### Issue tracker

Use GitHub Issues in `bradenbiz/silver-bible` for Wayfinder maps, decision tickets, specifications, and implementation work. Read [docs/agents/issue-tracker.md](docs/agents/issue-tracker.md), including its Wayfinding operations section, before using skills that interact with the tracker.

### Domain docs

Use a single context: `CONTEXT.md` and `docs/adr/` at the repository root. Read [docs/agents/domain.md](docs/agents/domain.md) for the consumer rules. Create domain documents as terms and decisions are resolved.

### Wayfinder

Use the installed Wayfinder skill when the user invokes it. This repository's tracker configuration is shared across computers and harnesses; the skills and CLI authentication must be available in each environment. See [README.md](README.md) for setup.
