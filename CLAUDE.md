# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an "awesome list" repository that curates and ranks open-source Python HTML generation projects. It uses the [best-of-generator](https://github.com/best-of-lists/best-of-generator) to automatically generate a ranked README from project metadata.

**Key insight**: This is NOT a code project. It's a metadata/content management repository where the README.md is auto-generated from `projects.yaml`.

## Architecture

```
projects.yaml          → Source of truth for all project metadata
     ↓
best-of-update-action  → GitHub Action that fetches metrics and generates README
     ↓
README.md              → Auto-generated ranked list (never edit directly)
```

**Template files:**

- `config/header.md` - README header template (supports variables: `{project_count}`, `{stars_count}`, `{category_count}`)
- `config/footer.md` - README footer template

## Working with This Repository

### Adding/Updating Projects

Edit `projects.yaml` only. Never modify README.md directly.

**Required project properties:**

- `name` - Unique project name
- `github_id` - Format: `owner/repo` (e.g., `pelme/htpy`)

**Optional properties:**

- `category` - One of: `html-generation`, `html-form-generation`, `related-projects`, `miscellaneous`
- `labels` - List of labels
- `pypi_id`, `conda_id`, `npm_id`, `dockerhub_id`, `maven_id` - Package manager IDs

**Example entry:**

```yaml
- name: htpy
  github_id: pelme/htpy
  category: "html-generation"
```

### Automated Updates

The GitHub workflow (`update-best-of-list.yml`) runs weekly:

1. Creates an `update/YYYY.MM.DD` branch
2. Fetches GitHub metrics and package manager data
3. Regenerates README.md with updated rankings
4. Creates a PR and draft release

Manual trigger: Run workflow from GitHub Actions with optional version input.

### Contribution Conventions

See `CONTRIBUTING.md` for full details. Key points:

- **One project per PR/issue**
- **Branch naming**: `feat/<github_id>` for projects, `docs/<description>` for docs
- **PR title**: `` Add `project:<github_id>` `` (use backtick-wrapped identifiers)
- **PR title with category**: `` Add `project:<github_id>` and `category:<id>` ``
- **Docs PR title**: `docs: <description>`
- **PR body**: Fill in `.github/PULL_REQUEST_TEMPLATE.md` — check the change type box, add a project link with description, check both checklist items
- **Commits**: Squash-merged using the PR title, so branch commit messages don't matter

## Files You Should Know

| File                | Purpose                                      |
| ------------------- | -------------------------------------------- |
| `projects.yaml`     | All project metadata - the only file to edit |
| `config/header.md`  | README header template                       |
| `config/footer.md`  | README footer template                       |
| `latest-changes.md` | Auto-generated trending projects summary     |
| `history/`          | Historical snapshots of each update          |
