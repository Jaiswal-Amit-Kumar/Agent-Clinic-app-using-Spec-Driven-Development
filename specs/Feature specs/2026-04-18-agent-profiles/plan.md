# Plan: Agent Profile Management

## Task Group 1: Database Schema & Migrations

1. Create migration: `agents` table with columns: `agent_id` (UUID, PK), `name` (TEXT NOT NULL), `model_type` (TEXT), `status` (TEXT CHECK: active/inactive/suspended DEFAULT 'active'), `registered_at` (TEXT NOT NULL ISO 8601), `last_seen` (TEXT)
2. Create migration: `ailments` table with columns: `ailment_id` (UUID, PK), `code` (TEXT UNIQUE NOT NULL, e.g. "HAL-001"), `name` (TEXT NOT NULL), `description` (TEXT), `category` (TEXT), `created_at` (TEXT NOT NULL)
3. Create migration: `agent_ailments` join table with columns: `agent_id` (FK → agents), `ailment_id` (FK → ailments), `severity` (INTEGER CHECK 1-4), `notes` (TEXT), `diagnosed_at` (TEXT), PRIMARY KEY (agent_id, ailment_id)
4. Create migration: `agent_ailments` index on `agent_id` and index on `ailment_id`
5. Add seed data: insert 5 sample ailments (HAL-001 Hallucination, CTX-001 Context Rot, DFT-001 Instruction Drift, PRS-001 Persona Collapse, RPT-001 Repetition Syndrome)
6. Add seed data: insert 5 sample agents with varied model types
7. Verify all migrations run cleanly with `npm run db:migrate`

## Task Group 2: API Routes

1. Create `GET /agents` route — list all agents with ailment count; supports `?status=` filter
2. Create `GET /agents/:id` route — return single agent with their ailments array (each ailment includes severity, notes, diagnosed_at)
3. Create `POST /agents` route — register a new agent; body: `{ name, model_type? }`; returns created agent with generated ID
4. Create `PATCH /agents/:id` route — update agent fields (name, model_type, status); body: `{ name?, model_type?, status? }`
5. Create `GET /ailments` route — list all available ailments
6. Create `POST /agents/:id/ailments` route — link an ailment to an agent; body: `{ ailment_id, severity, notes? }`; validates severity 1-4
7. Create `DELETE /agents/:id/ailments/:ailment_id` route — remove an ailment link from an agent
8. Add 404 responses for unknown agent/ailment IDs

## Task Group 3: Server Components / Pages

1. Create `src/pages/agents/index.tsx` — AgentList page: table of agents (name, model, status, ailment count), each row links to detail page
2. Create `src/pages/agents/[id]/index.tsx` — AgentDetail page: agent header (name, model, status, registered date), ailments list section (ailment name, severity badge, notes, diagnosed date), form to add new ailment (select ailment from dropdown, severity radio buttons, optional notes textarea)
3. Add navigation link to `/agents` from the site header nav
4. Style agent list and detail pages with the existing CSS custom properties (no new CSS framework)

## Task Group 4: Seed & Verify

1. Run seed script; verify agents page shows 5 agents
2. Verify clicking an agent shows their detail page with ailment list
3. Submit a new ailment via the form on the detail page; verify it appears in the list
4. Verify `GET /agents/:id` JSON response includes ailments array with severity
5. Mark Phase 2 "Agent profile management" as complete in `specs/roadmap.md`
