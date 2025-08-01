# gf-workflow-testing

## Release Management

### Creating a Release
1. Go to Actions → "Create Release or Hotfix"
2. Select "release" → Optionally specify commit SHA → Run workflow
3. Push commits to the generated `release/v*` branch (only if fixes needed)
4. **Deploy from release branch**: The Deploy workflow will run automatically with manual approval required
5. **Manually approve** the "Create GitHub Release" step in the Release Versioning workflow
6. Review and merge the auto-created PR (use "Create merge commit" - do not squash or rebase)

### Creating a Hotfix
1. Go to Actions → "Create Release or Hotfix"
2. Select "hotfix" → Run workflow
3. Push hotfix commits to the generated `hotfix/v*` branch
4. **Deploy from hotfix branch**: The Deploy workflow will run automatically with manual approval required
5. **Before merging PR**: Create GitHub release at generated URL with specified tag and branch
6. Merge the auto-created PR (use "Create merge commit" - do not squash or rebase)

### Deployment Process
- **Automatic Trigger**: Deploy workflow runs automatically when you push to `release/v*` or `hotfix/v*` branches
- **Manual Approval**: All deployments require manual approval via the "Release" environment in GitHub
- **Production Deployment**: Only happens if the commit has a release tag (semantic version)
- **Environment-Based**: Deploys to different environments based on branch type and release tag presence