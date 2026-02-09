# CrewAI-Studio - Installation Agent Brief

## Overview
This document provides the complete installation and configuration guide for CrewAI-Studio, a Streamlit-based GUI for the crewAI Python framework, configured to use DashScope Coding API as the OpenAI-compatible LLM provider.

## Prerequisites
- Docker and Docker Compose installed
- Git client available
- DashScope Coding API key for models `qwen3-coder-plus` or `qwen3-max-2026-01-23`
- Strong password generation capability

## Installation Steps

### 1. Clone Repository
```bash
git clone https://github.com/strnad/CrewAI-Studio.git
cd CrewAI-Studio
```

### 2. Configure Environment
```bash
cp .env_example .env
```

Edit `.env` with these required values:
- `POSTGRES_USER=crewai_user`
- `POSTGRES_PASSWORD=<generate-strong-password>`
- `POSTGRES_DB=crewai`
- `DB_URL=postgresql://crewai_user:<same-password>@db:5432/crewai`
- `OPENAI_API_KEY=<your-dashscope-coding-api-key>`
- `OPENAI_API_BASE=https://coding-intl.dashscope.aliyuncs.com/v1`

### 3. Build and Start
```bash
docker-compose up --build
```

### 4. Access Application
- Open http://localhost:8501 in your browser
- Verify the Streamlit interface loads completely

### 5. Initial Configuration Test
- Create a test agent with Role/Goal/Backstory
- Save and refresh to verify persistence
- Check LLM dropdown shows qwen3-coder-plus and qwen3-max-2026-01-23 models
- Create a simple task and assign to agent
- Create a crew with the agent and task
- Execute the crew and verify output displays

### 6. Data Persistence Verification
```bash
docker-compose down
docker-compose up
```
- Verify all previously created agents, tasks, and crews are still present
- Confirm database volume exists: `docker volume ls | grep db-data`

## Troubleshooting

### Port Conflicts
- If port 5432 is occupied: stop local PostgreSQL or modify docker-compose.yaml
- If port 8501 is occupied: modify docker-compose.yaml Streamlit port mapping

### API Key Issues
- Ensure `OPENAI_API_KEY` contains your DashScope Coding API key
- Ensure `OPENAI_API_BASE` is set to `https://coding-intl.dashscope.aliyuncs.com/v1`
- DashScope Coding API is the only supported LLM provider

### Data Loss Prevention
- **NEVER** run `docker-compose down -v` unless you want to delete all data
- Always use `docker-compose down` (without -v) to preserve data
- Run weekly database backups using the backup-db command

## Success Criteria
Installation is complete only when:
1. All 14 verification checklist items pass
2. Real crew execution produces visible output
3. Data persists through Docker restarts
4. DashScope models (qwen3-coder-plus, qwen3-max-2026-01-23) are available and functional

## Next Steps
After successful installation:
1. Create your first real crew using starter templates
2. Set up weekly database backup automation
3. Begin building your agent library for daily productivity using DashScope Coding models