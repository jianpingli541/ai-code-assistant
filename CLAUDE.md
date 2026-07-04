# CLAUDE.md - Project Configuration for Claude Code
# This file guides Claude Code on project structure, conventions, and rules.

## Project Overview
AI Code Assistant is a full-stack AI programming companion built with Supabase + Claude Code.

## Directory Structure
- `src/api/` - API route handlers
- `src/lib/` - Core library code (Supabase client, AI agent integration)
- `src/components/` - UI components
- `src/utils/` - Utility functions
- `supabase/migrations/` - Database migrations
- `tests/` - Test files
- `docs/` - Documentation

## Coding Conventions
- Use TypeScript strictly (no `any`)
- All API routes must have input validation
- Database queries must use typed parameters
- Error handling: always wrap in try/catch with specific error types
- Naming: camelCase for variables/functions, PascalCase for types/classes

## Database Rules
- All tables have RLS enabled
- Never bypass RLS in application code
- Use the generated TypeScript types from Supabase
- Migrations go in `supabase/migrations/`

## AI Agent Integration
- Use MCP protocol for tool integration
- Log all agent executions to `agent_executions` table
- Track tokens, cost, and duration for each execution
- Always validate agent output before applying changes

## Automation Pipeline
### Task-Skill Mapping
Skills are automatically matched to tasks based on labels and status:
- **code-review**: Auto-triggers on `in_progress` tasks with `backend`/`frontend`/`api`/`infra`/`devops` labels
- **test-generation**: Auto-triggers on tasks with `testing` label or title/description containing "test"
- **dependency-update**: Auto-triggers on `infra`/`devops` labels or high/critical priority `todo` tasks
- **doc-generation**: Auto-triggers on `done` tasks

### Trigger Flow
1. Task status changes → `on_task_status_change()` trigger fires
2. `auto_trigger_skills_for_task()` matches skills by label/priority/status
3. New `task_skills` record created with status `running`
4. Agent execution starts → `agent_executions` record created
5. Agent completes → `on_agent_execution_completed()` trigger fires
6. Task auto-updates to `done` when all associated skills complete

### External Callbacks
- Use `agent-callback` Edge Function to notify completion of external agent runs
- Endpoint: `https://<project>.supabase.co/functions/v1/agent-callback`
- Required body: `{ "execution_id": <id>, "status": "completed", "cost_estimate": <num>, "duration_seconds": <int>, "files_changed": <int> }`

### Dashboard Query
- Use `v_automation_pipeline` view for full pipeline visibility
- Query: `SELECT * FROM v_automation_pipeline ORDER BY task_id, skill_name;`

## Testing Requirements
- Unit tests for all utility functions
- Integration tests for API routes
- Minimum 80% code coverage
- Run tests before committing

## Security
- Never commit API keys or secrets
- Use environment variables for all configs
- Sanitize all user inputs
- Apply least-privilege principle for DB access
