# AI World

AI World is a visual multi-agent workspace where AI agents live inside a small interactive world instead of a traditional dashboard.

## Current version — v0.2

The project currently includes five specialized agents:

- **Commander** — understands the main goal, plans work and routes tasks to the team.
- **Coder** — implementation, debugging and technical work.
- **Researcher** — investigation, comparison and information gathering.
- **Designer** — product thinking, UI and UX work.
- **Reviewer** — checks the team's output and produces a final review.

## How it works

1. Enter a task in the command bar.
2. With **Auto Team** enabled, Commander asks Claude to create a work plan.
3. Relevant specialist agents run their assigned parts.
4. Reviewer receives the combined work and checks it against the original goal.
5. Results are displayed by agent in the Team Result panel.

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
- Agent/task/project data model
- Realtime-ready agent and task events
- Model and usage metadata support

The secret `ANTHROPIC_API_KEY` is stored only as a Supabase Edge Function secret.

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
   ↓
Commander / selected Agent
   ↓
Anthropic Claude API
   ↓
Specialist agents
   ↓
Reviewer
   ↓
Team Result
```

## Security

- Never commit the Anthropic API key to GitHub.
- `ANTHROPIC_API_KEY` belongs in Supabase Secrets only.
- The Supabase publishable/anon client key can be used by the browser, but database access must remain protected with RLS.
- AI World does not expose hidden model chain-of-thought; the interface displays useful statuses, actions and results instead.

## Next milestones

Planned improvements include Supabase Auth, cloud-synced agent positions/preferences, persistent project memory, task history, realtime activity, richer collaboration between agents, approval gates for important actions, and deeper GitHub integration.

## Status

**v0.2 — active development**

Frontend: `index.html`

Backend: Supabase + Claude API
