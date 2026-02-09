# CrewAI-Studio - Verification Checklist

Run every check. Report PASS or FAIL with actual output. Do not declare success unless ALL pass.

## Infrastructure Checks

| # | Check | Command | Expected |
|---|-------|---------|----------|
| 1 | Docker containers running | `docker-compose ps` | Both `crewai_studio` and `crewai_db` show "Up" |
| 2 | Streamlit UI responds | `curl -s -o /dev/null -w "%{http_code}" http://localhost:8501` | 200 |
| 3 | PostgreSQL healthy | `docker exec crewai_db pg_isready -U crewai_user` | "accepting connections" |
| 4 | Docker volume exists | `docker volume ls \| grep db-data` | Volume listed |

## Configuration Checks

| # | Check | Command | Expected |
|---|-------|---------|----------|
| 5 | OPENAI_API_KEY set (DashScope) | `grep OPENAI_API_KEY .env \| grep -v "^#"` | Non-empty DashScope API key |
| 6 | OPENAI_API_BASE configured | `grep OPENAI_API_BASE .env \| grep -v "^#"` | "https://coding-intl.dashscope.aliyuncs.com/v1" |
| 7 | DB_URL configured | `grep DB_URL .env \| grep -v "^#"` | Valid PostgreSQL URL |

## Functional Checks (manual via UI)

| # | Check | How | Expected |
|---|-------|-----|----------|
| 8 | Agent creation | Create agent with Role/Goal/Backstory → save → refresh | Agent persists |
| 9 | LLM selection | Check agent LLM dropdown | Shows qwen3-coder-plus and qwen3-max-2026-01-23 models |
| 10 | Task creation | Create task → assign agent → save | Task persists |
| 11 | Crew assembly | Create crew with agent + task → save | Crew persists |
| 12 | Crew execution | Run crew → wait for output | Output displays in UI |
| 13 | Results history | Check previous run results | Accessible after refresh |

## Persistence Check

| # | Check | How | Expected |
|---|-------|-----|----------|
| 14 | Data survives restart | `docker-compose down` then `docker-compose up` → check agents/crews | All data intact |

## Result

- Total checks: 14
- Passed: ___
- Failed: ___
- Status: PASS / FAIL