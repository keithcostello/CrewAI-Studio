---
description: Backup the CrewAI-Studio PostgreSQL database
---

Run a database backup:

```bash
docker exec crewai_db pg_dump -U crewai_user crewai > backup_$(date +%Y%m%d_%H%M%S).sql
```

Confirm the file was created, show its size, and remind me where it was saved.