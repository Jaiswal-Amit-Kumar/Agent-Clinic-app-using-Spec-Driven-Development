# Validation: Agent Profile Management

## How to Know the Implementation Succeeded

### Database
- [ ] Running `npm run db:migrate` creates the three tables without errors
- [ ] Running `npm run seed` inserts 5 ailments and 5 agents without errors
- [ ] `SELECT * FROM agents` returns 5 rows
- [ ] `SELECT * FROM ailments` returns 5 rows
- [ ] `SELECT * FROM agent_ailments` is initially empty

### API Routes

#### `GET /agents`
- [ ] Returns 200 with JSON array of 5 agents (each has name, model_type, status, registered_at)
- [ ] Response includes ailment_count per agent (computed)

#### `POST /agents`
- [ ] Returns 201 with created agent including generated agent_id
- [ ] Body `{ "name": "TestAgent", "model_type": "gpt-4o" }` creates agent
- [ ] Missing required `name` field returns 400

#### `GET /agents/:id`
- [ ] Returns 200 with agent object including `ailments` array (may be empty)
- [ ] Unknown ID returns 404

#### `PATCH /agents/:id`
- [ ] `{"status": "inactive"}` updates agent status
- [ ] Returns updated agent

#### `GET /ailments`
- [ ] Returns 200 with array of all 5 seeded ailments (each has id, code, name, category)

#### `POST /agents/:id/ailments`
- [ ] Returns 201 when linking an ailment; severity 1-4 accepted
- [ ] Re-linking the same ailment to the same agent returns 409 Conflict
- [ ] Invalid ailment_id returns 404

#### `DELETE /agents/:id/ailments/:ailment_id`
- [ ] Returns 204 on successful removal
- [ ] Removing a link that doesn't exist returns 404

### Pages

#### `/agents` (Agent List)
- [ ] Page loads without JS errors
- [ ] Table shows all agents with name, model, status, ailment count
- [ ] Each row links to `/agents/:id`

#### `/agents/:id` (Agent Detail)
- [ ] Agent header shows name, model, status, registered date
- [ ] Ailments section lists each linked ailment with severity badge, name, notes, diagnosed date
- [ ] "Add Ailment" form has: ailment dropdown (populated from GET /ailments), severity radio buttons (1-4), notes textarea, submit button
- [ ] Submitting the form links the ailment and it appears in the list immediately after page reload
- [ ] Empty ailment list shows a helpful empty state ("No ailments recorded")

### Integration
- [ ] After creating an agent via POST, it appears in the list page
- [ ] After adding an ailment via the form, the ailment count on the list page increments
- [ ] After deleting an ailment, the list updates on next page load

### Definition of Done
All checkboxes above are checked. The feature is merged when:
1. All API routes return correct status codes and shapes
2. Both pages render correctly with server-side data
3. No TypeScript errors in `npm run typecheck`
4. `npm test` passes (if tests exist for this phase)
