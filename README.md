# GitHub Workflow Testing Repository

This repository demonstrates a comprehensive automated release management system using GitHub Actions, featuring semantic versioning, CI/CD pipelines, and deployment automation.

## Overview

The workflow system supports two types of releases:
- **Regular Releases**: Cut from the main branch for new features and improvements
- **Hotfixes**: Cut from the latest release tag for urgent fixes

Both processes use semantic versioning and automated deployments while maintaining clear separation between development and production environments.

## Workflow Files

### Continuous Integration (`ci-cd.yml`)
- **Triggers**: All pull requests + pushes to `main` branch
- **Jobs**:
  - `lint`: Code quality checks
  - `unit-test`: Automated unit testing
  - `ui-test`: Browser automation tests
  - `dev-deploy`: Automatic deployment to development environment (main branch only)
- **Purpose**: Ensures code quality and automatically deploys successful main branch changes

### Release Management

#### Branch Creation
- **`create-release-branch.yml`**: Creates release branches from main (or specified commit)
- **`create-hotfix-branch.yml`**: Creates hotfix branches from the latest release tag

#### Release Processing
- **`create-release-pull-request.yml`**: Automatically runs semantic-release and creates pull requests for release branches
- **`create-hotfix-release.yml`**: Manual approval workflow for hotfix versioning using semantic-release

#### Production Deployment
- **`deploy-to-production.yml`**: Manual deployment workflow using git tags

## Release Process

### Creating a Regular Release

1. **Create Release Branch**
   - Go to Actions → "Create Release Branch" → Run workflow
   - Optionally specify a commit SHA (defaults to main branch tip)
   - Creates branch named `release/vYYYY-MM-DD-{short-sha}`

2. **Automatic Processing**
   - Push any additional commits to the release branch if needed
   - `create-release-pull-request.yml` automatically runs semantic-release
   - Semantic-release updates version, changelog, and creates git tag
   - Pull request is automatically created targeting main branch

3. **Deploy to Production**
   - Use the generated git tag to run `deploy-to-production.yml` workflow
   - If additional patches are needed, push commits to release branch
   - New semantic-release run will create updated tag for deployment

### Creating a Hotfix

1. **Create Hotfix Branch**
   - Go to Actions → "Create Hotfix Branch" → Run workflow
   - Creates branch named `hotfix/v{next-patch-version}` from latest release tag
   - Branch name indicates the target version (e.g., `hotfix/v2.8.1`)

2. **Manual Release Process**
   - Push hotfix commits to the hotfix branch
   - Create a pull request manually targeting main branch
   - Approve the most recent run of `create-hotfix-release.yml` workflow
   - This runs semantic-release to bump version, update changelog, and create git tag

3. **Deploy to Production**
   - Use the generated git tag to run `deploy-to-production.yml` workflow

## Deployment Environments

### Development Environment
- **Trigger**: Automatic on every push to `main` branch
- **Workflow**: `ci-cd.yml` → `dev-deploy` job
- **Requirements**: All tests must pass first
- **Approval**: None (fully automated)

### Production Environment
- **Trigger**: Manual execution of `deploy-to-production.yml`
- **Input**: Requires a git tag (e.g., `v2.8.0`)
- **Validation**: Ensures tag exists and follows semantic versioning
- **Scope**: Full production deployment with versioned assets

## Key Features

- **Semantic Versioning**: Automatic version bumping based on conventional commits
- **Path-based Ignoring**: CI/CD skips runs for documentation-only changes
- **Tag-based Deployment**: Production deployments are tied to specific git tags
- **Environment Separation**: Clear distinction between dev and production workflows
- **Manual Approval Gates**: Hotfix releases require manual approval for safety
- **Asset Management**: Automated Docker builds and S3 asset publishing