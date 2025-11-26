# command-env-check

A Claude Code slash command for verifying development environment setup.

## Installation

```bash
# Clone to your preferred location
git clone git@github.com:claude-commands/command-env-check.git <clone-path>/command-env-check

# Symlink (use full path to cloned repo)
ln -s <clone-path>/command-env-check/env-check.md ~/.claude/commands/env-check.md
```text

## Usage

```text
/env-check              # Full environment verification
/env-check --tools      # Check required tools only
/env-check --env        # Check environment variables only
/env-check --services   # Check service connections only
```text

## What it does

1. Detects project type and requirements
2. Verifies required tools and versions
3. Checks environment variables against examples
4. Tests database and service connections
5. Verifies dependencies are installed
6. Generates setup report with fix commands

## Output Format

```markdown
# Environment Check Report

## Tools
| Tool | Required | Installed | Status |
|------|----------|-----------|--------|
| Node.js | 18.x | 18.17.0 | ✅ |

## Environment Variables
| Variable | Status |
|----------|--------|
| DATABASE_URL | ✅ Set |
| STRIPE_KEY | ❌ Missing |

## Services
| Service | Status |
|---------|--------|
| PostgreSQL | ✅ Connected |
| Redis | ❌ Not Running |

## Quick Setup
```bash
docker-compose up -d
npm install
```text

```text

## Status Icons

| Icon | Meaning |
|------|---------|
| ✅ | Correct/Running |
| ⚠️ | Warning |
| ❌ | Error/Missing |

## Requirements

- Git repository with source code
- Claude Code with Opus 4.5 model access

## Updates

```bash
cd <clone-path>/command-env-check && git pull
```text
