# Auto-Update Setup Guide

## Problem
The app is getting this error when checking for updates:
```
Error: ENOENT: no such file or directory, open 'C:\Users\ahoin\AppData\Local\WeeDesktopLauncher\app-1.9.4\resources\app-update.yml'
```

## Root Cause
The `app-update.yml` file is missing from the published version. This file is generated by Electron Forge during the publishing process and contains information about where to find updates.

## Solution

### 1. Update Version
First, increment the version in `package.json`:
```json
{
  "version": "1.9.6"
}
```

### 2. Ensure Proper Forge Configuration
The `forge.config.js` file has been updated with:
- `generateUpdatesFilesForAllChannels: true` in packagerConfig
- Proper GitHub publisher configuration
- Removed duplicate configuration from `package.json`

### 3. Publish with Auto-Update Support
Run the release script to properly build and publish:
```bash
npm run release
```

This script will:
1. Build the app (`npm run build`)
2. Package the app (`npm run package`)
3. Publish to GitHub (`npm run publish`)

### 4. Verify the Release
After publishing, check that:
- The GitHub release includes the `app-update.yml` file
- The release assets include the Windows installer
- The version number matches what's in `package.json`

### 5. Test Auto-Update
Users with the current version (1.9.4) should now be able to auto-update to the new version (1.9.6).

## Debugging

### Check Current Status
Run the check-update script to diagnose issues:
```bash
npm run check-update
```

### Manual Verification
1. Download the new version from GitHub
2. Install it
3. Check if `app-update.yml` exists in the app directory
4. Test the auto-update functionality

## Important Notes

### For Future Releases
- Always use `npm run release` instead of manual publishing
- Ensure the version is incremented before publishing
- The `app-update.yml` file will be automatically generated

### For Users
- The first published version (1.9.4) will show an auto-update error
- This is expected and will be resolved with the next release
- Users can manually download updates from GitHub until auto-update is working

## Troubleshooting

### If Auto-Update Still Doesn't Work
1. Check GitHub repository settings
2. Ensure the repository is public or the GitHub token has proper permissions
3. Verify the release assets are properly uploaded
4. Check the `app-update.yml` file contents

### Common Issues
- **Missing GitHub token**: Set `GH_TOKEN` environment variable
- **Repository permissions**: Ensure the token has `repo` scope
- **Release assets**: Make sure all files are uploaded to the GitHub release

## Files Modified
- `forge.config.js` - Added auto-update configuration
- `package.json` - Removed duplicate Forge config, added scripts
- `electron.cjs` - Improved error handling for missing app-update.yml
- `scripts/release.cjs` - New release script
- `scripts/check-update.cjs` - New debugging script 