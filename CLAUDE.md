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