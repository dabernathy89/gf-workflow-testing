# gf-workflow-testing

## Release Management

### Creating a Release
1. Go to Actions → "Create Release or Hotfix"
2. Select "release" → Run workflow
3. Push commits to the generated `release/v*` branch (only if fixes needed)
4. **Manually approve** the "Create GitHub Release" step in Actions
5. Review and merge the auto-created PR (use "Create merge commit" or "Rebase and merge")

### Creating a Hotfix
1. Go to Actions → "Create Release or Hotfix" 
2. Select "hotfix" → Run workflow
3. Push hotfix commits to the generated `hotfix/v*` branch
4. **Before merging PR**: Create GitHub release at generated URL with specified tag and branch
5. Merge the auto-created PR (use "Create merge commit" or "Rebase and merge")