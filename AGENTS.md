# CrewAI-Studio - Agent Instructions

## Project Overview

This is a fork of strnad/CrewAI-Studio, a Streamlit-based GUI for the crewAI Python framework. It runs as a Docker Compose stack (Streamlit app + PostgreSQL) and is used by two people (Keith and Amy) as a daily workbench for creating AI agent teams, assigning them work, and reviewing results.

**This is a productivity tool, not a demo.** Treat the agent library as a growing asset. Data persistence matters.

## Tech Stack

- Python (Streamlit frontend + crewAI backend)
- PostgreSQL (data persistence via Docker volume)
- Docker / Docker Compose (deployment)
- crewAI framework (agent orchestration)
- LLM providers: DashScope Coding (qwen3-coder-plus, qwen3-max-2026-01-23) as OpenAI-compatible API

## Critical Rules

1. **Docker only.** Do not install via Conda or venv. All changes must work within the Docker Compose stack.
2. **Never delete Docker volumes** without explicit confirmation. `docker-compose down -v` destroys all agent/crew data.
3. **DashScope Coding is the primary LLM.** Always verify API key is configured for `https://coding-intl.dashscope.aliyuncs.com/v1` with models `qwen3-coder-plus` or `qwen3-max-2026-01-23`.
4. **No UI overhauls.** This is Streamlit. Don't try to make it React. Functional > pretty.
5. **No auth layers, no multi-tenancy, no external API integrations** beyond LLM providers.
6. **Test with real crew execution** before declaring any change complete. A crew must run end-to-end.

## Build & Run

```bash
# Clone and configure
git clone https://github.com/strnad/CrewAI-Studio.git
cd CrewAI-Studio
cp .env_example .env
# Edit .env - see Environment section below

# Build and run
docker-compose up --build

# Access
# http://localhost:8501

# Stop (preserves data)
docker-compose down

# Stop AND DELETE ALL DATA (dangerous)
docker-compose down -v
```

## Environment Configuration

Required `.env` entries:

```env
POSTGRES_USER=crewai_user
POSTGRES_PASSWORD=<generate-strong-password>
POSTGRES_DB=crewai
DB_URL=postgresql://crewai_user:<same-password>@db:5432/crewai
OPENAI_API_KEY=<your-dashscope-coding-api-key>
OPENAI_API_BASE=https://coding-intl.dashscope.aliyuncs.com/v1
```

## Database Backup

```bash
docker exec crewai_db pg_dump -U crewai_user crewai > backup_$(date +%Y%m%d).sql
```

Run this weekly. The agent library accumulates value over time.

## Port Conflicts

If port 5432 is already in use by a local PostgreSQL, either stop it or remap in docker-compose.yaml. Port 8501 is Streamlit - if occupied, remap similarly.

## Verification After Any Change

After any modification, run through:
1. UI loads at http://localhost:8501
2. Create agent → save → refresh → persists
3. LLM dropdown shows Anthropic Claude models
4. Create task → assign agent → save
5. Create crew → add agent + task → save
6. Execute crew → output displays
7. Results survive `docker-compose down` + `docker-compose up`

If any step fails, the change is not complete.