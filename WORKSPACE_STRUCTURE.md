# Workspace Structure - RE Automation GmbH

> Local development environment structure and setup guide

## Overview

```
C:\WORKSPACE_LOCAL\              <- Local development (fast SSD)
        |
        | git push/pull
        v
GITHUB (REAutomation)            <- Single Source of Truth
        |
        | Mirror script (TrueNAS cron)
        v
TRUENAS (Z:\_git-mirrors\)       <- Disaster Recovery
        |
        | GoodSync
        v
SHAREPOINT                       <- Cloud Redundancy
```

---

## Local Workspace: C:\WORKSPACE_LOCAL\

### Core Principles

| Principle | Description |
|-----------|-------------|
| **Flat structure** | No deep nesting, all projects at root level |
| **Naming convention** | See [REPO_NAMING.md](REPO_NAMING.md) |
| **Git-optimized** | Every project is a Git repo |
| **Local = Fast** | Active development only locally, not on network drive |

### Folder Categories

```
C:\WORKSPACE_LOCAL\
|
+-- [Customer Projects]
|   +-- asys-25043-bosbra-codesys\
|   +-- pps-25044-workflow-hmi-web\
|   +-- zeiser-24140-hbsb-codesys\
|   +-- ...
|
+-- [Libraries]
|   +-- lib-motion-codesys\
|   +-- lib-base-codesys\
|   +-- ...
|
+-- [Internal Tools]
|   +-- internal-servo-tool\
|   +-- internal-codesys-automation\
|   +-- internal-multifab-control\
|   +-- ...
|
+-- [Special Folders - NO GIT]
|   +-- _sandbox\           <- Experiments, tests, playground
|   +-- _temp\              <- Temporary files
|   +-- _archive\           <- Local archive copies
|   +-- _tools\             <- Installed tools, IDEs
|
+-- README.md               <- Overview for new team members
```

### Naming Convention (Summary)

| Type | Pattern | Example |
|------|---------|---------|
| Customer project | `[customer]-[YYNNN]-[part]-[stack]` | `mahle-25001-main-codesys` |
| Library | `lib-[name]-[stack]` | `lib-motion-codesys` |
| Internal tool | `internal-[name]` | `internal-servo-tool` |
| Template | `template-[name]-[stack]` | `template-machine-codesys` |

**Stack Suffixes:**
- `-codesys` - CODESYS V3.5 PLC
- `-tia` - Siemens TIA Portal
- `-hmi-web` - React/Web visualization
- `-backend` - Node.js server/API
- `-embedded` - Firmware/Microcontroller
- `-docs` - Documentation

> Full convention: [REPO_NAMING.md](REPO_NAMING.md)

---

## Network Drive: Z:\ (TrueNAS)

### Structure

```
Z:\  (\\192.168.2.230\RE-Shared)
|
+-- Development\
|   +-- _git-mirrors\           <- Automatic GitHub backups (bare clones)
|   |   +-- [repo-name].git\
|   |   +-- sync.log
|   |   +-- last-sync.txt
|   |
|   +-- _archive\               <- Completed projects
|   |   +-- 2024\
|   |   +-- 2025\
|   |   +-- 2026\
|   |
|   +-- _shared-resources\      <- Shared resources (no Git)
|       +-- vendor-libs\        <- Vendor libraries
|       +-- documentation\      <- Manuals, datasheets
|
+-- Projects\
|   +-- 01_Client Projects\     <- Project documentation (not code!)
|   +-- 02_Internal Projects\
|
+-- Software\
+-- CRM\
+-- Accounting\
+-- ...
```

### Important Rules for Z:\

| Rule | Reason |
|------|--------|
| **NO active Git on Z:\** | Slow, ownership issues |
| **Only backups/mirrors** | `_git-mirrors` are bare clones |
| **No development** | Always work locally, then push |

---

## GitHub: REAutomation

### Organization

**URL:** https://github.com/REAutomation

### Repository Visibility

| Type | Visibility |
|------|------------|
| Customer projects | `private` |
| Internal tools | `private` |
| Libraries | `private` or `internal` |
| Templates | `private` |
| .github | `public` (for README) |

### Topics for Filtering

Every repo should have relevant topics:
- Customer: `customer-asys`, `customer-mahle`, `customer-zeiser`
- Technology: `codesys`, `tia`, `embedded`, `web`
- Type: `library`, `template`, `tool`
- Year: `2024`, `2025`, `2026`

---

## Backup Chain

### Automatic Flow

```
1. Developer works locally (C:\WORKSPACE_LOCAL\)
2. git push -> GitHub (Single Source of Truth)
3. Nightly: Mirror script on TrueNAS fetches all repos
4. GoodSync syncs Z:\ -> SharePoint
```

### Mirror Script (TrueNAS Cron, daily 02:00)

```bash
#!/bin/bash
MIRROR_DIR="/mnt/pool/Development/_git-mirrors"
ORG="REAutomation"

for repo in $(gh repo list $ORG --json name -q '.[].name'); do
    if [ -d "$MIRROR_DIR/$repo.git" ]; then
        git -C "$MIRROR_DIR/$repo.git" fetch --all --prune
    else
        git clone --mirror "https://github.com/$ORG/$repo.git" "$MIRROR_DIR/$repo.git"
    fi
done

echo "$(date): Mirror completed" >> "$MIRROR_DIR/sync.log"
```

---

## New Workspace Setup Guide

### 1. Create Folder Structure

```powershell
mkdir C:\WORKSPACE_LOCAL
mkdir C:\WORKSPACE_LOCAL\_sandbox
mkdir C:\WORKSPACE_LOCAL\_temp
mkdir C:\WORKSPACE_LOCAL\_archive
```

### 2. Configure Git

```powershell
git config --global user.name "Firstname Lastname"
git config --global user.email "email@re-automation.de"
git config --global init.defaultBranch main
```

### 3. Install GitHub CLI

```powershell
winget install GitHub.cli
gh auth login  # Browser OAuth
gh auth status # Verify
```

### 4. Clone Project

```powershell
cd C:\WORKSPACE_LOCAL
gh repo clone REAutomation/[repo-name]
```

### 5. Create New Project

```powershell
cd C:\WORKSPACE_LOCAL
mkdir [project-name-per-convention]
cd [project-name-per-convention]
git init

# Create GitHub repo and push
gh repo create REAutomation/[project-name] --private --source=. --push

# Set topics
gh repo edit REAutomation/[project-name] --add-topic customer-xxx,codesys,2025
```

---

## Project Migration Checklist

When migrating an existing project:

```
[ ] Backup of original folder created
[ ] Project number identified (YYNNN from ERP)
[ ] New name per convention determined
[ ] Folder renamed
[ ] .git present? If not: git init
[ ] .gitignore present and correct?
[ ] Temporary files removed (node_modules, __pycache__, etc.)
[ ] GitHub repo created (private)
[ ] Remote added: git remote add origin ...
[ ] Initial push: git push -u origin main
[ ] Topics set
[ ] README.md present?
[ ] Old folder deleted or archived
```

---

## Common Issues

### Git Ownership Error on Network Drive

```
fatal: detected dubious ownership in repository
```

**Solution:** Don't work on network drive. Clone locally, work there.

### Slow Git Operations

**Cause:** Git on network drive or very large repos

**Solution:**
- Work locally
- Check `.gitignore` (don't commit build artifacts)
- Git LFS for large binary files

### Project Without ERP Number

For projects in quotation phase or internal projects:
- Customer projects: Placeholder `[customer]-00000-[part]-[stack]`
- Rename after order confirmation
- Internal: `internal-[name]`

---

*Document Version: 1.1*
*Date: 2026-01-15*
*Maintainer: RE Automation GmbH*
