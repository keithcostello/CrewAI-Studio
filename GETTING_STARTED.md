# CrewAI-Studio with DashScope Coding API - Getting Started Guide

This guide will help you set up and run your customized CrewAI-Studio instance configured to use the DashScope Coding API as your LLM provider.

## Prerequisites

1. **Docker Desktop** installed and running on your system
2. **Git** installed
3. **DashScope Coding API key** (already configured in your `.env` file)

## Step-by-Step Setup Instructions

### 1. Start Docker Desktop
Before running any Docker commands, ensure Docker Desktop is running:
- Open the Docker Desktop application on Windows
- Wait for the Docker whale icon to appear in your system tray
- Verify Docker is running by executing: `docker info`

> **Note**: The error "unable to get image 'postgres:15': error during connect..." occurs when Docker Desktop is not running. Always start Docker Desktop first!

### 2. Navigate to Project Directory
```bash
cd C:\Projects\OpenCode_02072026\crewai-studio
```

### 3. Verify Your Configuration
Check that your `.env` file contains the correct DashScope settings:
```bash
cat .env
```

You should see these key configurations:
```
OPENAI_API_KEY="sk-sp-9e4c19448060451ab7ea18a05e716c04"
OPENAI_API_BASE=https://coding-intl.dashscope.aliyuncs.com/v1
```

### 4. Start the Application
```bash
docker-compose up --build
```

This command will:
- Pull necessary Docker images (including PostgreSQL 15)
- Build the Streamlit application container
- Start both the database and web application containers

> **Note**: You may see a warning about the `version` attribute in docker-compose.yaml being obsolete. This is just a warning and can be safely ignored.

### 5. Access the Application
Once the containers are running successfully, open your web browser and navigate to:
```
http://localhost:8501
```

## Verification Checklist

After accessing the UI, verify everything is working correctly:

### Basic Functionality
- [ ] UI loads completely at http://localhost:8501
- [ ] Create an agent with Role/Goal/Backstory → save → refresh → persists
- [ ] LLM dropdown shows `qwen3-coder-plus` and `qwen3-max-2026-01-23` models
- [ ] Create a task → assign agent → save → persists
- [ ] Create a crew → add agent + task → save → persists
- [ ] Execute crew → output displays in UI
- [ ] Previous run results are accessible after refresh

### Data Persistence
- [ ] Stop containers with `Ctrl+C`
- [ ] Restart with `docker-compose up`
- [ ] All previously created agents, tasks, and crews are still present
- [ ] Database volume exists: `docker volume ls | grep db-data`

## Using OpenCode.ai Commands (Optional)

If you have OpenCode.ai integrated with this repository, you can use these helpful commands:

- **`/verify`** - Runs the complete 14-point verification checklist automatically
- **`/new-crew`** - Guides you through creating a crew from the 4 starter templates
- **`/backup-db`** - Creates a timestamped database backup

## Database Backup (Recommended)

To protect your growing agent library, set up regular database backups:

```bash
# Create a backup with timestamp
docker exec crewai_db pg_dump -U crewai_user crewai > backup_$(date +%Y%m%d_%H%M%S).sql

# Verify backup was created
ls -la backup_*.sql
```

Run this weekly or before making significant changes.

## Troubleshooting

### Common Issues and Solutions

**Issue: Docker daemon not running**
- **Error**: `unable to get image 'postgres:15': error during connect...`
- **Solution**: Start Docker Desktop application and wait for it to fully initialize

**Issue: Port conflicts**
- **Symptoms**: Containers fail to start due to port binding errors
- **Solution**: 
  - For PostgreSQL (port 5432): Stop local PostgreSQL service or modify `docker-compose.yaml`
  - For Streamlit (port 8501): Change port mapping in `docker-compose.yaml`

**Issue: LLM models not appearing in dropdown**
- **Symptoms**: Only default models shown, no DashScope models
- **Solution**: 
  - Verify `.env` file has correct `OPENAI_API_KEY` and `OPENAI_API_BASE`
  - Ensure your DashScope API key has access to coding models
  - Check Docker logs: `docker-compose logs web`

**Issue: Data loss concerns**
- **Critical**: NEVER run `docker-compose down -v` (this permanently deletes all data)
- **Safe**: Always use `docker-compose down` to stop containers (preserves data)
- **Verify**: Use `docker volume ls` to confirm `db-data` volume exists

### Checking Logs
If you encounter issues, check the application logs:
```bash
# View web application logs
docker-compose logs web

# View database logs  
docker-compose logs db

# Follow logs in real-time
docker-compose logs -f web
```

## Next Steps

1. **Explore Starter Templates**: Review the 4 pre-built crew templates in `docs/starter-crews.md`
2. **Create Your First Real Crew**: Use one of the templates as a starting point
3. **Customize Agents**: Adapt roles, goals, and backstories for your specific needs
4. **Set Up Automation**: Create scripts for regular database backups
5. **Test Model Performance**: Compare `qwen3-coder-plus` vs `qwen3-max-2026-01-23` for your use cases

## Important Notes

- **Docker Only**: This setup uses Docker exclusively - no Conda or venv installations
- **Data Protection**: Your agent library accumulates value over time - protect it with backups
- **DashScope Primary**: `qwen3-coder-plus` and `qwen3-max-2026-01-23` are your only LLM options
- **Verification Required**: Always test end-to-end crew execution before considering setup complete

Your CrewAI-Studio is now ready to serve as a powerful daily workbench for AI agent team orchestration using the DashScope Coding API!