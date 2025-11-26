---
argument-hint: "[focus]"
description: "Verify development environment setup and dependencies"
model: claude-opus-4-5-20251101
allowed-tools: ["Bash", "Read", "Glob", "Grep", "AskUserQuestion"]
---

**If `$ARGUMENTS` is empty or not provided:**

Run comprehensive environment check for the current project.

**Usage:** `/env-check [focus]`

**Examples:**

- `/env-check` - Full environment verification
- `/env-check --tools` - Check required tools only
- `/env-check --env` - Check environment variables only
- `/env-check --services` - Check service connections only

**Workflow:**

1. Detect project type and requirements
2. Verify required tools and versions
3. Check environment variables
4. Test service connections
5. Generate setup report

Proceed with full environment check.

---

**If `$ARGUMENTS` is provided:**

Run focused environment check.

## Configuration

- **Focus**: `$ARGUMENTS`
  - `--tools`: Required tools and versions
  - `--env`: Environment variables
  - `--services`: Database, cache, etc.
  - `--deps`: Dependencies installed correctly

## Steps

1. **Detect Project Type**

   Check for:

   ```bash
   # Identify project type
   ls package.json go.mod requirements.txt Cargo.toml pyproject.toml Gemfile 2>/dev/null
   ```text

   Determine:
   - Primary language (Node, Go, Python, Rust)
   - Framework (React, Express, Django, etc.)
   - Infrastructure (Docker, K8s, etc.)

2. **Read Project Requirements**

   Check documentation:
   - `README.md` - Setup instructions
   - `.nvmrc`, `.node-version` - Node version
   - `.python-version` - Python version
   - `.tool-versions` - asdf versions
   - `docker-compose.yml` - Required services

3. **Verify Required Tools**

   **Core Tools:**

   ```bash
   # Git
   git --version

   # Package managers
   npm --version 2>/dev/null
   yarn --version 2>/dev/null
   pnpm --version 2>/dev/null
   go version 2>/dev/null
   python --version 2>/dev/null
   pip --version 2>/dev/null
   cargo --version 2>/dev/null
   ```text

   **Common Dev Tools:**

   ```bash
   # Docker
   docker --version 2>/dev/null
   docker-compose --version 2>/dev/null

   # Database CLIs
   psql --version 2>/dev/null
   mysql --version 2>/dev/null
   mongosh --version 2>/dev/null

   # Cloud CLIs
   aws --version 2>/dev/null
   gcloud --version 2>/dev/null
   az --version 2>/dev/null
   ```text

4. **Check Version Requirements**

   Compare installed vs required:

   ```bash
   # Node version from .nvmrc
   cat .nvmrc 2>/dev/null
   node --version

   # Go version from go.mod
   grep "^go " go.mod 2>/dev/null
   go version
   ```text

   Flag mismatches:
   - Major version difference = Critical
   - Minor version difference = Warning
   - Patch difference = Info

5. **Verify Environment Variables**

   Check for `.env.example` or documentation:

   ```bash
   # Find example env file
   cat .env.example 2>/dev/null || cat .env.sample 2>/dev/null
   ```text

   Compare with actual `.env`:

   ```bash
   # Check which vars are set
   cat .env 2>/dev/null | grep -v '^#' | cut -d= -f1
   ```text

   Identify:
   - Missing required variables
   - Variables with placeholder values
   - Sensitive variables not set

6. **Test Service Connections**

   **Database:**

   ```bash
   # PostgreSQL
   pg_isready -h localhost -p 5432 2>/dev/null

   # MySQL
   mysqladmin ping -h localhost 2>/dev/null

   # MongoDB
   mongosh --eval "db.stats()" --quiet 2>/dev/null

   # Redis
   redis-cli ping 2>/dev/null
   ```text

   **Check from env vars:**

   ```bash
   # Test DATABASE_URL connection
   # Parse and test connection string
   ```text

7. **Verify Dependencies**

   **Node.js:**

   ```bash
   # Check if node_modules exists and matches lockfile
   npm ls --depth=0 2>/dev/null
   npm ci --dry-run 2>/dev/null
   ```text

   **Go:**

   ```bash
   # Verify modules
   go mod verify 2>/dev/null
   ```text

   **Python:**

   ```bash
   # Check installed packages
   pip check 2>/dev/null
   ```text

8. **Check Docker Status**

   ```bash
   # Is Docker running?
   docker info 2>/dev/null

   # Are required containers running?
   docker-compose ps 2>/dev/null
   ```text

9. **Generate Environment Report**

   ```markdown
   # Environment Check Report

   **Project**: my-app
   **Type**: Node.js / Express
   **Status**: ⚠️ Issues Found

   ## System Requirements

   | Tool | Required | Installed | Status |
   |------|----------|-----------|--------|
   | Node.js | 18.x | 18.17.0 | ✅ |
   | npm | 9.x | 9.6.7 | ✅ |
   | Git | any | 2.39.0 | ✅ |
   | Docker | any | 24.0.2 | ✅ |
   | PostgreSQL | 14+ | 15.2 | ✅ |

   ## Environment Variables

   | Variable | Status | Notes |
   |----------|--------|-------|
   | DATABASE_URL | ✅ Set | |
   | REDIS_URL | ✅ Set | |
   | JWT_SECRET | ⚠️ Placeholder | Using default value |
   | STRIPE_KEY | ❌ Missing | Required for payments |

   **Missing Variables:**
   ```bash
   # Add to .env:
   STRIPE_KEY=sk_test_...
   ```text

   ## Service Connections

   | Service | Status | Details |
   |---------|--------|---------|
   | PostgreSQL | ✅ Connected | localhost:5432 |
   | Redis | ✅ Connected | localhost:6379 |
   | Elasticsearch | ❌ Not Running | Port 9200 refused |

   **Start services:**

   ```bash
   docker-compose up -d elasticsearch
   ```text

   ## Dependencies

   | Check | Status | Details |
   |-------|--------|---------|
   | node_modules | ✅ | Up to date |
   | Lockfile | ✅ | package-lock.json exists |
   | Audit | ⚠️ | 3 moderate vulnerabilities |

   ## Issues Found

   ### 1. Missing Environment Variable

   - **Variable**: `STRIPE_KEY`
   - **Required for**: Payment processing
   - **Fix**: Add to `.env` file

   ### 2. Elasticsearch Not Running

   - **Port**: 9200
   - **Fix**: `docker-compose up -d elasticsearch`

   ### 3. JWT_SECRET Using Default

   - **Risk**: Security vulnerability in production
   - **Fix**: Generate secure secret: `openssl rand -hex 32`

   ## Quick Setup Commands

   ```bash
   # Install dependencies
   npm install

   # Copy env file
   cp .env.example .env

   # Start services
   docker-compose up -d

   # Run migrations
   npm run db:migrate

   # Start dev server
   npm run dev
   ```text

   ## Verification

   After fixing issues, run:

   ```bash
   npm run dev
   # or
   npm test
   ```text

   ```text

10. **Provide Fix Commands**

    For each issue, provide:
    - Specific fix command
    - Installation instructions
    - Configuration guidance

## Output Structure

```markdown
# Environment Check Report

## Summary
[Overall status and issue count]

## Tools
[Required tools and versions]

## Environment Variables
[Missing or misconfigured vars]

## Services
[Database, cache, etc. status]

## Issues
[Problems found with fixes]

## Quick Setup
[Commands to get running]
```text

## Status Icons

| Icon | Meaning |
|------|---------|
| ✅ | Correct/Running/Set |
| ⚠️ | Warning (works but needs attention) |
| ❌ | Error/Missing/Failed |
| ℹ️ | Info (optional item) |

## Common Checks by Project Type

| Type | Tools | Services |
|------|-------|----------|
| Node.js | node, npm/yarn | MongoDB, Redis |
| Go | go, make | PostgreSQL |
| Python | python, pip | PostgreSQL, Redis |
| Rails | ruby, bundler | PostgreSQL, Redis |

## Notes

- Run this before `npm install` or starting dev
- Compare with CI environment for parity
- Production env vars should never appear locally
- Use `.env.local` for personal overrides
- Docker Desktop must be running for containers
