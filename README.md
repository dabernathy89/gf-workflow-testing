# gf-workflow-testing

## GitHub Workflows Overview

This repository demonstrates an automated release management system with semantic versioning, CI/CD pipelines, and deployment automation using GitHub Actions.

### Workflow Files

#### CI/CD Pipeline (`ci-cd.yml`)
- **Triggers**: Pull requests to any branch + pushes to `main`
- **Jobs**: 
  - `lint`, `unit-test`, `ui-test` (run on all PRs)
  - `dev-deploy` (only runs on pushes to main after tests pass)
- **Purpose**: Continuous integration testing and automatic development environment deployment

#### Release Branch Creation
- **`create-release-branch.yml`**: Manual workflow to create release branches from main
- **`create-hotfix-branch.yml`**: Manual workflow to create hotfix branches from latest release tag

#### Release Processing
- **`create-release-pull-request.yml`**: Runs semantic-release and creates PRs for `release/v*` branches
- **`create-hotfix-pull-request.yml`**: Creates PRs for `hotfix/v*` branches (no semantic-release)

#### Production Release & Deploy (`release-and-deploy.yml`)
- **Triggers**: Pull requests from `release/v*` or `hotfix/v*` branches to main
- **Environment**: Requires manual approval via "Release" environment
- **Jobs**:
  - `create-github-release` (only for release branches, hotfixes require manual release creation)
  - `deploy` (production deployment)

## Release Management Process

### Creating a Release
1. **Create Branch**: Go to Actions → "Create Release Branch" → Run workflow
2. **Make Changes**: Push commits to the generated `release/v*` branch (if needed)
3. **Auto Processing**: `create-release-pull-request.yml` runs semantic-release and creates PR
4. **Production Deploy**: PR triggers `release-and-deploy.yml` with manual approval required
5. **Merge**: Use "Create merge commit" (preserve git tags for deployment)

### Creating a Hotfix
1. **Create Branch**: Go to Actions → "Create Hotfix Branch" → Run workflow
2. **Make Changes**: Push hotfix commits to the generated `hotfix/v*` branch
3. **Auto PR**: `create-hotfix-pull-request.yml` creates PR with manual release instructions
4. **Manual Release**: Create GitHub release before merging (as instructed in PR)
5. **Production Deploy**: PR triggers `release-and-deploy.yml` with manual approval required
6. **Merge**: Use "Create merge commit"

## Deployment Environments

### Development
- **Trigger**: Automatic on every push to `main` branch
- **Workflow**: `ci-cd.yml` → `dev-deploy` job
- **Approval**: None required (automatic after tests pass)

### Production
- **Trigger**: Pull requests from release/hotfix branches
- **Workflow**: `release-and-deploy.yml`
- **Approval**: Manual approval required via "Release" environment
- **Scope**: Full production deployment with versioned releases