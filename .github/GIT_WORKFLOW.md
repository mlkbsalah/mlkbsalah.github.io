# Git Workflow

This document outlines the conventions for using Git and writing commit messages in this project.

## Commit Message Format

All commit messages should follow this format:

```
<type>: <subject>

<body (optional)>
```

**Types:**

- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code (white-space, formatting, etc.)
- `config`: Changes to configuration files
- `chore`: Changes to the build process or auxiliary tools and libraries

**Examples:**

```
feat: Add dark mode toggle button to header
fix: Correct baseurl in project site configuration
docs: Update INSTALL.md with Docker troubleshooting
style: Format all Liquid templates with Prettier
config: Enable blog section in _config.yml
chore: Update Jekyll dependencies with bundle update --all
```

## Local Pre-Commit Setup

Install the repository's Node dependencies and activate the Git hooks once per clone
(requires Node.js and `pre-commit`):

```bash
npm ci
pre-commit install --install-hooks
```

Each commit runs the existing whitespace, YAML, and large-file checks, followed by
Prettier on staged text files. Prettier uses the installed version and Liquid plugin
from `package-lock.json`, along with `.prettierrc` and `.prettierignore`, matching CI.
Unsupported file formats are skipped.

If a hook formats files, the commit stops. Review the changes, stage the corrected
files explicitly with `git add`, and retry the commit. Hooks do not automatically
stage changes. After dependency updates, run `npm ci` again.

To check formatting across the repository, including files outside your commit:

```bash
npx prettier . --check
```

## Staging Changes

**Always `git add` files explicitly.** Do not stage everything with `git add .` unless you are certain of what's being committed. Check `git status` first to review your changes.

## What NOT to Commit

**Always obey the project's [`.gitignore`](../.gitignore) file.** It prevents the accidental commit of:

- Build outputs (`_site/`, `.jekyll-cache/`)
- Dependencies (`node_modules/`, `vendor/`)
- OS-specific files (`.DS_store`)
- Editor temporary files (`.idea/`, `.swp`, `.swo`)
- Secrets and API keys (never commit credentials)
