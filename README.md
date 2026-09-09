# AI World

AI World is a visual multi-agent workspace where AI agents live inside a small interactive world instead of a traditional dashboard.

## Current version — v0.2

The project currently includes five specialized agents:

- **Commander** — understands the main goal, plans work and routes tasks to the team.
- **Coder** — implementation, debugging and technical work.
- **Researcher** — investigation, comparison and information gathering.
- **Designer** — product thinking, UI and UX work.
- **Reviewer** — checks the team's output and produces a final review.

## Connected workspace

AI World is now configured to work with the **AI Learning Path** project in GitHub:

`mayangabinet-create/-`

GitHub policy:

- Agents may read the project from `main`.
- Code changes must be made on branches named `ai-world/*`.
- Coder may prepare file changes and commits on those branches.
- AI World may open a pull request back to `main`.
- Agents are not allowed to push directly to `main`.
- Human approval is required before merge.

The workspace policy is also stored in `aiworld.config.json`.

## How it works

1. Enter a task in the command bar.
2. With **Auto Team** enabled, Commander asks Claude to create a work plan.
3. Relevant specialist agents run their assigned parts.
4. For coding work, Coder can inspect the connected AI Learning Path repository and prepare changes on an `ai-world/*` branch.
5. Reviewer receives the combined work and checks it against the original goal.
6. Proposed code changes are delivered through a GitHub pull request for human review.
7. Results are displayed by agent in the Team Result panel.

You can also tap an individual agent and run it directly.

## Model selection

AI World does not force every agent to use the same Claude model.

- Set a **Default Model** for the whole world.
- Override the model for any individual agent.
- Add custom instructions for each agent.
- Reset an agent back to the world default at any time.

The Anthropic API key is never stored in the frontend. Model requests go through the Supabase Edge Function.

## Backend

Backend project: **Ai ChatGPT** on Supabase.

Current backend pieces include:

- Supabase Postgres
- Edge Function: `ai-world-claude`
- Anthropic API integration
- GitHub repository read support for `mayangabinet-create/-`
- GitHub branch / file write / PR endpoints prepared for `ai-world/*` branches
- Agent/task/project data model
- Realtime-ready agent and task events
- Model and usage metadata support

Secrets stay on the Supabase Edge Function side:

- `ANTHROPIC_API_KEY` — Claude access
- `GITHUB_TOKEN` — GitHub write access for the connected repository

`GITHUB_TOKEN` is optional for public read access, but it is required before Coder can create branches, commit changes, or open pull requests.

## GitHub tools exposed to AI World

The backend currently supports these GitHub actions for the connected project:

- Repository status
- List repository files/folders
- Read files
- Create an `ai-world/*` branch
- Create or update files on that branch
- Open a pull request to `main`

Direct writes to `main` are blocked by the Edge Function.

## Interface

The world contains five work areas:

- Command HQ
- Code District
- Research Lab
- Design Studio
- Review Station

Agents can be dragged around the world. The UI includes mobile touch handling for iPhone/iPad as well as pointer input for desktop browsers.

## Architecture

```text
User task
   ↓
AI World frontend
   ↓
Supabase Edge Function
   ├── Claude API
   │      ↓
   │   AI agents
   │
   └── GitHub API
          ↓
   mayangabinet-create/-
          ↓
   ai-world/* branch
          ↓
      Pull Request
          ↓
     Human approval
```

## Security

- Never commit Anthropic or GitHub secrets to GitHub.
- `ANTHROPIC_API_KEY` belongs in Supabase Secrets only.
- `GITHUB_TOKEN` belongs in Supabase Secrets only.
- GitHub writes are restricted to `mayangabinet-create/-`.
- GitHub writes are restricted to branches beginning with `ai-world/`.
- Direct writes to `main` are blocked.
- Merge remains a human approval action.
- The Supabase publishable/anon client key can be used by the browser, but database access must remain protected with RLS.
- AI World does not expose hidden model chain-of-thought; the interface displays useful statuses, actions and results instead.

## Next milestones

The next step is adding `GITHUB_TOKEN` to the Supabase Edge Function secrets. After that, the Coder Agent can use the prepared GitHub workflow end-to-end: inspect project → create branch → change code → commit → open PR → Reviewer checks → user decides whether to merge.

Other planned improvements include Supabase Auth, cloud-synced agent positions/preferences, persistent project memory, task history, realtime activity and richer collaboration between agents.

## Status

**v0.2 — active development**

Frontend: `index.html`

Backend: Supabase + Claude API + GitHub integration
