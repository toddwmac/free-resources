# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**MyPages** is a static site generator for hosting and indexing files on GitHub Pages. It automatically lists any files containing `-AppliedAILabs` in their filename, specifically designed to showcase AI collaborative whitepapers and AI-enhanced content examples from Center For Applied AI / Applied AI Labs.

## Architecture

The project uses a **dual-index approach**:

1. **Dynamic Index** (`index.html` - default): A JavaScript-based index that fetches the file list from the GitHub API at runtime. This approach requires no pre-processing but needs JavaScript enabled in browsers.

2. **Static Index**: A pre-generated HTML index created by `scripts/update_index.py` that doesn't require JavaScript or API calls. This is automatically generated and committed when files change (via GitHub Actions workflow).

### Key Components

- **`index.html`**: The main landing page. It serves as the dynamic index by default and fetches files via GitHub API. Contains a Center For Applied AI logo header and instructions about confirming resources/references.

- **`scripts/update_index.py`**: Python script that generates a static version of the index by:
  - Listing all git-tracked files using `git ls-files`
  - Filtering for files containing `-AppliedAILabs` in their filename
  - Building static HTML with links to all matching files
  - Only updating `index.html` if content changed (prevents unnecessary commits)

- **`.github/workflows/update-index.yml`**: GitHub Actions workflow that:
  - Runs on every push to `main` branch
  - Executes the Python update script
  - Automatically commits and pushes changes (labeled `[skip ci]` to prevent recursive runs)
  - Only runs if the commit author is not the `github-actions[bot]` itself

## Common Commands

### Regenerate Static Index
```bash
python scripts/update_index.py
```
Run this after adding or removing content files to update `index.html`. The GitHub Actions workflow handles this automatically on push.

### Add a New Content File
1. Create or add your file to the repository root
2. Ensure the filename contains `-AppliedAILabs` (any file type is supported)
3. Commit and push to `main` branch
4. GitHub Actions will automatically regenerate the index

## File Naming Convention

- **Required text**: Files must contain `-AppliedAILabs` in their filename to appear in the index
- Any file type is supported (`.html`, `.pdf`, `.docx`, etc.)
- Files are sorted alphabetically in the listing
- `index.html` itself is excluded from the file list

## Development Notes

- **Python version**: Uses Python 3 (no specific version requirement)
- **Git dependency**: The index generator uses `git ls-files`, so git must be available in the environment
- **HTML escaping**: The Python script properly HTML-escapes file paths to prevent injection issues
- **No build step**: This is a static site generator with no compilation or bundling required
