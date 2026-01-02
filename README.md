# Confluence Documents Sync

Automatically sync Markdown files to Confluence pages using GitHub Actions.

## Overview

This repository manages documentation that is automatically synchronized to Confluence. Write your documentation in Markdown, commit to the repository, and the content is automatically published to your Confluence space.

## Features

- Write documentation in standard Markdown format
- Automatic synchronization to Confluence on push to main branch
- PR validation with linting
- Support for images and media files
- Version control for your documentation

## Configuration

### cosmere.json

The `cosmere.json` file maps your local Markdown files to Confluence pages:

```json
{
  "baseUrl": "https://your-domain.atlassian.net/wiki/rest/api",
  "cachePath": "build",
  "prefix": "This document is automatically generated. Please don't edit it directly!",
  "pages": [
    {
      "pageId": "360554",
      "file": "README.md",
      "title": "Sync Markdown files with Confluence"
    }
  ]
}
```

**Configuration options:**
- `baseUrl`: Your Confluence API endpoint
- `cachePath`: Directory for build artifacts (should be in .gitignore)
- `prefix`: Message shown at the top of auto-generated Confluence pages
- `pages`: Array of page mappings
  - `pageId`: The Confluence page ID to update
  - `file`: Path to the local Markdown file
  - `title`: Page title in Confluence

## Setup

### Prerequisites

- Node.js 20.x or higher
- Access to a Confluence instance
- Confluence API token

### Local Development

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd confluence-documents
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Configure your Confluence credentials:
   ```bash
   export CONFLUENCE_USERNAME="your-email@example.com"
   export CONFLUENCE_TOKEN="your-api-token"
   ```

4. Run linting:
   ```bash
   npm run lint
   ```

5. Publish to Confluence:
   ```bash
   npm run publish
   ```

### GitHub Actions Setup

1. Add the following secrets to your GitHub repository:
   - `CONFLUENCE_USERNAME`: Your Confluence email address
   - `CONFLUENCE_TOKEN`: Your Confluence API token (not password!)

2. The workflows will automatically:
   - Run linting on all pull requests
   - Sync to Confluence when changes are pushed to main

### Getting a Confluence API Token

1. Log in to https://id.atlassian.com/manage-profile/security/api-tokens
2. Click "Create API token"
3. Give it a name and copy the token
4. Use this token (not your password) in `CONFLUENCE_TOKEN`

## Project Structure

```
.
├── .github/
│   └── workflows/        # GitHub Actions workflows
├── docs/                 # Documentation files
│   ├── media/           # Images and media files
│   ├── demo1.md
│   └── demo2.md
├── cosmere.json         # Confluence sync configuration
├── package.json         # Node.js dependencies
└── README.md           # This file
```

## Adding New Pages

1. Create your Markdown file in the appropriate directory
2. Create a new page in Confluence and note its page ID
3. Add an entry to `cosmere.json`:
   ```json
   {
     "pageId": "123456",
     "file": "docs/my-new-page.md",
     "title": "My New Page"
   }
   ```
4. Commit and push your changes

## Markdown Support

This sync tool supports standard GitHub-flavored Markdown including:
- Headers
- Lists (ordered and unordered)
- Code blocks with syntax highlighting
- Tables
- Images
- Links
- Bold, italic, and strikethrough text
- Blockquotes

## Troubleshooting

### Authentication Errors
- Ensure you're using an API token, not your password
- Verify your username is your Confluence email address
- Check that the token has not expired

### Page Not Found
- Verify the page ID in `cosmere.json` is correct
- Ensure you have edit permissions on the Confluence page

### Build Failures
- Run `npm run lint` locally to catch issues before pushing
- Check the GitHub Actions logs for detailed error messages

## Contributing

1. Create a new branch for your changes
2. Make your updates to the Markdown files
3. Submit a pull request
4. The PR will be automatically validated
5. Once merged, changes will sync to Confluence

## License

MIT
