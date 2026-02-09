---
description: Run the full post-install verification checklist
agent: verifier
---

Run the CrewAI-Studio verification checklist:

1. Check Docker containers are running: `docker-compose ps`
2. Check UI responds: `curl -s -o /dev/null -w "%{http_code}" http://localhost:8501`
3. Check PostgreSQL is accepting connections: `docker exec crewai_db pg_isready`
4. Check .env has ANTHROPIC_API_KEY set (not empty)
5. Check .env has OPENAI_API_KEY set (not empty)
6. Check Docker volume exists: `docker volume ls | grep db-data`
7. Report PASS/FAIL for each check with the actual output.