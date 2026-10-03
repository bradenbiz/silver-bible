# Silver Bible

Silver Bible is a project in the planning stage, with [bible.ag](https://bible.ag) as its domain on Vercel.

## Planning

Use [GitHub Issues](https://github.com/bradenbiz/silver-bible/issues) for Wayfinder maps, decision tickets, specifications, and implementation work.

[Matt Pocock's Wayfinder](https://github.com/mattpocock/skills/tree/main/skills/engineering/wayfinder) guides the planning process. A map is an issue labelled `wayfinder:map`; its decision tickets are GitHub sub-issues with native blocking dependencies.

The product direction and technology stack will be decided during planning.

Agent instructions live in [AGENTS.md](AGENTS.md). The shared [issue tracker configuration](docs/agents/issue-tracker.md) defines maps, child tickets, blocking relationships, and the frontier query. [Domain documentation rules](docs/agents/domain.md) describe where terminology and architectural decisions belong.

## Working from another computer or harness

Clone this repository, or pull `main` in an existing checkout:

```sh
git clone https://github.com/bradenbiz/silver-bible.git
cd silver-bible
gh auth login
```

Install [Matt Pocock's Wayfinder and its supporting skills](https://github.com/mattpocock/skills) in the harness you use on that computer. Skill installations and CLI logins are local to each environment; cloning this repo provides the shared configuration, not the skills themselves. Ensure the harness reads `AGENTS.md`; if it does not load that file automatically, explicitly provide it as project instructions.

Invoke Wayfinder with your idea to start a map, or with an existing map's URL to continue. The repo is already configured to use GitHub Issues, so setup does not need to be rerun after cloning. You can edit `docs/agents/*.md` directly as conventions change.

Wayfinder planning needs GitHub access, not Vercel credentials. Neither `.vercel/` nor `.env.local` is required for planning, and both stay out of Git.

When you need local Vercel development or deployment, install the Vercel CLI and run from the checkout:

```sh
vercel login
vercel link --project silver-bible --scope bradenbizs-projects --yes
vercel env pull .env.local --environment development --scope bradenbizs-projects
```

Linking recreates `.vercel/`; pulling retrieves the project's development environment values. Obtain any Vercel OIDC token through that environment's authenticated Vercel tooling rather than copying another computer's token. The project has no application-specific environment requirements yet. Hosted Vercel deployments use environment values configured on Vercel itself.
