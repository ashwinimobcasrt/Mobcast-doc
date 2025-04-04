# Git Branching Process

## 1. Overview

This document outlines the Git branching strategy for efficient product development using Bitbucket. It ensures structured development, QA validation, and smooth deployment to production while maintaining Jira integration.

### Key Branches:
- **common-code-master**: Production-ready code
- **common-code-dev**: Ongoing development
- **release**: QA-approved changes ready for production

## 2. Workflow for New Features

### Step 1: Create a Jira Ticket
Every new feature starts with a Jira ticket (e.g., DS-1234). Assign the ticket to the developer.

### Step 2: Create a Feature Branch
Branch from common-code-master:
```bash
git checkout common-code-master
git pull origin common-code-master
git checkout -b feature/DS-1234-feature-name
```

Naming convention: `feature/DS-<JIRA-ID>-short-description`

### Step 3: Development & Commits
Commit changes with Jira reference:
```bash
git commit -m "[DS-1234] Implemented new API endpoint"
git push origin feature/DS-1234-feature-name
```

### Step 4: Create a Pull Request (PR)
PR should be from `feature/DS-1234-feature-name` → `common-code-dev`.

### Step 5: Code Review & Merge to Dev
Code is reviewed and merged into common-code-dev. Feature is tested in common-code-dev before moving to QA.

### Step 6: Move to Release Branch for QA Approval
Once QA approves a feature, it is cherry-picked or merged into release:
```bash
git checkout release
git pull origin release
git merge --no-ff feature/DS-1234-feature-name
git push origin release
```

### Step 7: Release to Production
Create a PR from `release` → `common-code-master`. After approval, merge into common-code-master and deploy.

## 3. Workflow for Hotfixes

### Step 1: Create a Jira Ticket
A Jira ticket is mandatory for every hotfix (e.g., DS-5678).

### Step 2: Create a Hotfix Branch from Master
Hotfixes should be based on common-code-master:
```bash
git checkout common-code-master
git pull origin common-code-master
git checkout -b hotfix/DS-5678-critical-bugfix
```

### Step 3: Fix & Commit Changes
Commit with Jira reference:
```bash
git commit -m "[DS-5678] Fixed payment gateway timeout issue"
```

### Step 4: Create a Pull Request (PR) & Merge to Master
PR should be from `hotfix/DS-5678-critical-bugfix` → `common-code-master`.

### Step 5: Merge Hotfix Back to Dev & Release
To avoid conflicts, merge the hotfix into both common-code-dev and release:
```bash
git checkout release
git merge --no-ff hotfix/DS-5678-critical-bugfix
git push origin release

git checkout common-code-dev
git merge --no-ff hotfix/DS-5678-critical-bugfix
git push origin common-code-dev
```

###  4. Additional Best Practices
- **Branch Protection Rules**: Require PR reviews and CI/CD checks for automated testing.
- **Jira Automation**: Auto-update Jira statuses upon merging.
- **Feature Flags**: Use feature toggles to control deployments.