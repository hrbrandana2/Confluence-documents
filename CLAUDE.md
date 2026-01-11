# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository manages documentation that automatically synchronizes Markdown files to Confluence pages using the `cosmere` CLI tool. Documentation is written in standard Markdown, committed to Git, and published to Confluence either manually or via GitHub Actions.

## Common Commands

### Development Workflow
```bash
# Install dependencies
npm install
# or for clean install (CI environments)
npm ci

# Lint markdown files
npm run lint

# Auto-fix markdown linting issues
npm run lint:fix

# Publish to Confluence (requires credentials)
npm run publish
```

### Required Environment Variables
Before running `npm run publish`, you must set:
```bash
export CONFLUENCE_USERNAME="your-email@example.com"
export CONFLUENCE_TOKEN="your-api-token"
```

Note: Use an API token, not your Confluence password. Generate tokens at: https://id.atlassian.com/manage-profile/security/api-tokens

## Architecture

### Core Components

**cosmere.json** - Central configuration file that maps local Markdown files to Confluence pages:
- `baseUrl`: Confluence API endpoint (format: `https://{domain}.atlassian.net/wiki/rest/api`)
- `cachePath`: Build artifacts directory (should be in .gitignore)
- `prefix`: Banner message displayed at top of auto-generated Confluence pages
- `pages`: Array of page mappings with `pageId`, `file`, and `title` fields

**Documentation Structure**:
- `docs/` - Main documentation directory containing Markdown files
- `docs/media/` - Images and media files referenced in documentation
- `README.md` - Can also be synced to Confluence

### Publishing Flow

1. Markdown files are written/edited in the repository
2. `npm run publish` (or cosmere CLI) reads `cosmere.json` configuration
3. For each entry in the `pages` array:
   - Reads the local Markdown file
   - Converts to Confluence storage format
   - Updates the specified Confluence page via REST API
4. Media files in `docs/media/` are uploaded and linked appropriately

### GitHub Actions Workflows

**Sync-Confluence-Documentation.yml** (Manual trigger):
- Triggered via `workflow_dispatch`
- Runs linting, then publishes to Confluence
- Uses repository secrets: `CONFLUENCE_USERNAME`, `CONFLUENCE_TOKEN`, `CONFLUENCE_PASSWORD`

**check-pr.yml** (Automatic on PRs):
- Runs on pull requests to `main` branch
- Validates markdown files using `npm run lint`
- Does NOT publish to Confluence

## Adding New Documentation Pages

1. Create the Markdown file (e.g., `docs/new-page.md`)
2. Create a new page in Confluence and note its page ID (found in the page URL)
3. Add an entry to `cosmere.json`:
   ```json
   {
     "pageId": "123456",
     "file": "docs/new-page.md",
     "title": "My New Page"
   }
   ```
4. Test locally with `npm run lint` and `npm run publish`
5. Commit and push changes

## Linting Rules

- Uses `markdownlint-cli` with default rules
- README.md has `MD013` (line length) disabled for flexibility
- All files in `pages/**/*.md` are linted (Note: Currently using `docs/` directory instead)

## Node.js Requirements

- Minimum version: Node.js 20.0.0 or higher (specified in `package.json` engines)
- Package manager: npm (with package-lock.json)
