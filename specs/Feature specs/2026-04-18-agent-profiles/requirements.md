# Requirements: Agent Profile Management

## Feature Summary

Agents can have multiple ailments, each with an independent severity level (1-4). This enables a realistic model where an agent can simultaneously suffer from, say, Hallucination (severity 3) and Context Rot (severity 1).

## Scope

### Delivers
- `agents` database table with persistent agent identity
- `ailments` database table with seeded catalog of AI agent ailments
- `agent_ailments` join table supporting multiple ailments per agent with per-ailment severity
- REST API routes for CRUD on agents and linking/unlinking ailments
- Agent list page (`/agents`) showing all agents with ailment counts
- Agent detail page (`/agents/:id`) showing full profile with ailment list and add-ailment form

### Does Not Deliver
- Dashboard analytics (that's Phase 3/4)
- Appointment booking (separate phase)
- Any LLM-powered diagnosis (simplified — ailments are assigned manually via form)
- Treatment recommendations (separate phase)

## Data Model

### agents
| Field | Type | Notes |
|-------|------|-------|
| agent_id | UUID | PK, generated at creation |
| name | TEXT NOT NULL | e.g. "ResearchBot-7" |
| model_type | TEXT | e.g. "claude-sonnet-4-20250514" |
| status | TEXT | CHECK IN (active, inactive, suspended), DEFAULT 'active' |
| registered_at | TEXT | ISO 8601, set at creation |
| last_seen | TEXT | ISO 8601, updated on each API call |

### ailments (seeded, not user-editable in this phase)
| Field | Type | Notes |
|-------|------|-------|
| ailment_id | UUID | PK |
| code | TEXT UNIQUE | e.g. "HAL-001" |
| name | TEXT NOT NULL | e.g. "Hallucination" |
| description | TEXT | |
| category | TEXT | e.g. "Output Integrity" |
| created_at | TEXT | ISO 8601 |

### agent_ailments (join table)
| Field | Type | Notes |
|-------|------|-------|
| agent_id | UUID | FK → agents.agent_id |
| ailment_id | UUID | FK → ailments.ailment_id |
| severity | INTEGER | CHECK 1-4, required |
| notes | TEXT | Optional context, e.g. " worsening over last 3 days" |
| diagnosed_at | TEXT | ISO 8601, set when link is created |

**Composite PK**: (agent_id, ailment_id) — same agent can't be linked to the same ailment twice.

## Routes

| Method | Path | Description |
|--------|------|-------------|
| GET | /agents | List all agents |
| POST | /agents | Register new agent |
| GET | /agents/:id | Get agent with ailments |
| PATCH | /agents/:id | Update agent |
| GET | /ailments | List all available ailments |
| POST | /agents/:id/ailments | Link ailment to agent |
| DELETE | /agents/:id/ailments/:ailment_id | Unlink ailment from agent |

## Severity Scale

| Level | Label | Meaning |
|-------|-------|---------|
| 1 | MILD | Minor degradation, functional |
| 2 | MODERATE | Noticeably impaired |
| 3 | SEVERE | Core function affected |
| 4 | CRITICAL | Non-functional or harmful |

## Tech Stack Alignment

- **Hono** for routing and middleware
- **Hono JSX** for server-side page rendering (no client framework)
- **SQLite via better-sqlite3** — plain SQL migrations, no ORM in this phase
- **Vitest** for any new tests
- **tsx** for development server

## Constraints

- No ORM — raw SQL for schema and queries
- Server-side rendering only — no client-side JS for this feature
- All state changes via REST API (no WebSocket, no SSE in this phase)
