# CLAUDE.md - AI Assistant Guide for GitHub Docs Repository

This document provides comprehensive guidance for AI assistants working with the GitHub Docs codebase. Last updated: 2025-11-18

## Table of Contents

- [Repository Overview](#repository-overview)
- [Quick Start for AI Assistants](#quick-start-for-ai-assistants)
- [Technology Stack](#technology-stack)
- [Directory Structure](#directory-structure)
- [Development Workflows](#development-workflows)
- [Code Conventions](#code-conventions)
- [Content Structure & Authoring](#content-structure--authoring)
- [Testing](#testing)
- [Build & Deployment](#build--deployment)
- [Contributing Guidelines](#contributing-guidelines)
- [Common Tasks](#common-tasks)
- [Key Files Reference](#key-files-reference)

---

## Repository Overview

**Purpose**: Official documentation site for GitHub (https://docs.github.com)

**Repository Structure**:
- **Public repo** (`github/docs`): Open to external contributions
- **Private repo** (`github/docs-internal`): For GitHub employees
- **Sync**: Changes sync bidirectionally between repos

**Licensing**:
- Content (`/assets`, `/content`, `/data`): Creative Commons Attribution 4.0
- Code: MIT License

**Scope**:
- GitHub.com features
- GitHub Enterprise Server/Cloud
- GitHub Actions, Copilot, CLI, Desktop, Mobile
- Code Security, Codespaces, REST/GraphQL APIs
- 8 languages: English, Spanish, Japanese, Portuguese, Chinese, Russian, French, Korean, German
- 39 major documentation categories

---

## Quick Start for AI Assistants

### Before Making Changes

1. **Understand the scope**: This is the PUBLIC repo. Only modify:
   - ✅ Content files (`.md` in `/content`)
   - ✅ Select data files (`/data/reusables`, `/data/variables`)
   - ❌ Infrastructure, workflows, build code (not accepted from external contributors)

2. **Check versioning**: Content may need to support multiple GitHub versions
   - `fpt`: Free/Pro/Team (GitHub.com)
   - `ghes`: Enterprise Server (version-specific)
   - `ghec`: Enterprise Cloud
   - `ghae`: Azure (deprecated)

3. **Test locally**: Always test changes before committing
   ```bash
   npm ci                    # Install dependencies
   npm run build             # Build Next.js app
   npm start                 # Start dev server (localhost:4000)
   npm run lint-content      # Lint content files
   ```

### File Location Guide

| Task | Location | Example |
|------|----------|---------|
| Edit article | `/content/{category}/{subcategory}/{article}.md` | `/content/actions/learn-github-actions/understanding-github-actions.md` |
| Add reusable text | `/data/reusables/{category}/{topic}.md` | `/data/reusables/actions/workflow-basics.md` |
| Add variable | `/data/variables/{category}.yml` | `/data/variables/product.yml` |
| Fix UI component | `/src/frame/components/` | `/src/frame/components/Header.tsx` |
| Update search | `/src/search/` | `/src/search/lib/search.ts` |
| Add/modify tests | `/src/{subject}/tests/` | `/src/content-render/tests/` |

---

## Technology Stack

### Runtime & Framework
- **Node.js**: v22 or v24
- **Framework**: Next.js 15.4.7 (App Router)
- **Language**: TypeScript (ES2022 target, strict mode)
- **Server**: Express.js 5.1.0
- **UI**: React 18.3.1 + Primer React 37.31.0

### Content Processing
- **Markdown**: Unified/Remark/Rehype ecosystem
- **Templating**: LiquidJS 10.16.7
- **HTML Parsing**: Cheerio 1.0.0-rc.12
- **Syntax Highlighting**: highlight.js

### Search & Data
- **Search**: Elasticsearch 8.13.1
- **Validation**: Custom content linter (in `/src/content-linter`)

### Testing & Quality
- **Unit/Integration**: Vitest 3.1.2
- **E2E**: Playwright 1.56
- **Linting**: ESLint with TypeScript, Primer, a11y plugins
- **Formatting**: Prettier (100 char width, single quotes, no semicolons)

### Build & Deploy
- **Build**: tsx 4.19.4, Next.js build
- **Container**: Docker multi-stage build
- **Platform**: Moda (GitHub's internal platform)
- **CDN**: Fastly

---

## Directory Structure

```
/home/user/docs-et-cet-alia/
│
├── .github/               # GitHub Actions workflows & custom actions
│   ├── workflows/         # 50+ CI/CD workflows
│   ├── actions/           # Custom composite actions
│   └── CONTRIBUTING.md    # External contributor guide
│
├── .vscode/               # VSCode settings & recommended extensions
│
├── assets/                # Static assets (images, fonts)
│   └── images/            # Screenshots, diagrams
│
├── config/                # Kubernetes & Moda deployment configs
│   └── moda/              # Production deployment configuration
│
├── content/               # 📝 DOCUMENTATION CONTENT (Markdown)
│   ├── get-started/
│   ├── actions/
│   ├── admin/
│   ├── code-security/
│   ├── copilot/
│   ├── rest/              # REST API docs (59 subdirectories)
│   ├── graphql/
│   ├── webhooks/
│   └── [36 more categories...]
│
├── contributing/          # Contributor documentation
│   ├── content-markup-reference.md
│   ├── content-style-guide.md
│   ├── content-templates.md
│   └── [20+ guide files]
│
├── data/                  # 📊 STRUCTURED DATA
│   ├── features/          # Feature-based versioning
│   ├── reusables/         # Long reusable text blocks (Markdown)
│   ├── variables/         # Short reusable strings (YAML)
│   ├── glossaries/        # Term definitions
│   ├── learning-tracks/   # Guided learning paths
│   ├── release-notes/     # Version release notes
│   ├── graphql/           # GraphQL schema data
│   └── ui.yml             # Localized UI strings
│
├── src/                   # 💻 APPLICATION SOURCE CODE (45 modules)
│   ├── app/               # Next.js app router pages
│   ├── frame/             # Header, footer, global layout
│   ├── content-render/    # Markdown/Liquid → HTML pipeline
│   ├── content-linter/    # Content validation engine
│   ├── search/            # Search functionality & Elasticsearch
│   ├── rest/              # REST API documentation system
│   ├── graphql/           # GraphQL API documentation
│   ├── webhooks/          # Webhook documentation
│   ├── languages/         # Translation/i18n support
│   ├── tests/             # Shared test utilities
│   ├── fixtures/          # Test fixtures
│   ├── workflows/         # Build automation scripts
│   └── [35+ more modules...]
│
├── translations/          # Translation files (cloned at build time)
│
├── package.json           # Dependencies & scripts
├── tsconfig.json          # TypeScript configuration
├── next.config.ts         # Next.js configuration
├── eslint.config.ts       # ESLint configuration
├── vitest.config.ts       # Vitest test configuration
├── .editorconfig          # Editor settings (2 spaces, UTF-8)
├── .env.example           # Environment variables template
├── Dockerfile             # Production build
└── README.md              # Getting started guide
```

### Subject Folder Pattern

Each feature is organized as a self-contained module:

```
src/{subject}/
  ├── README.md           # Module documentation
  ├── lib/                # Core business logic
  ├── middleware/         # Express middleware
  ├── components/         # React components
  ├── scripts/            # CLI scripts
  └── tests/              # Module-specific tests
```

**Example**: `src/search/`
- `lib/search.ts` - Core search logic
- `middleware/search.ts` - API route handler
- `components/Search.tsx` - Search UI
- `scripts/index-elasticsearch.ts` - Index builder
- `tests/search.test.ts` - Tests

---

## Development Workflows

### Setup & Running

```bash
# Clone and install
git clone https://github.com/github/docs
cd docs
npm ci                    # Install exact dependencies from lock file

# Development
npm run build             # Build Next.js app (required before first start)
npm start                 # Start dev server on localhost:4000
# or
npm run dev               # Alias for npm start

# Enable only English (faster startup)
ENABLED_LANGUAGES=en npm start

# Production build
NODE_ENV=production npm run build
npm run start-for-production
```

### Key Scripts

| Script | Command | Purpose |
|--------|---------|---------|
| `start` | `npm start` | Development server (localhost:4000) |
| `build` | `npm run build` | Next.js production build |
| `test` | `npm test` | Run all tests with Vitest |
| `lint` | `npm run lint` | Lint TypeScript/TSX files |
| `lint-content` | `npm run lint-content` | Lint content Markdown files |
| `prettier` | `npm run prettier` | Format code with Prettier |
| `fix` | `npm run fix` | Auto-fix lint issues |

### Git Workflow

1. **Branch**: Create feature branch from `main`
   ```bash
   git checkout -b feature/my-change
   ```

2. **Make changes**: Edit content or code

3. **Test**: Run tests and linters
   ```bash
   npm test
   npm run lint
   npm run lint-content
   ```

4. **Commit**: Pre-commit hooks will auto-format and lint
   - Hooks run ESLint on `.ts`/`.tsx`
   - Prettier on `.scss`/`.ts`/`.tsx`/`.yml`/`.yaml`
   - Content linter on `.md` in `/content` and `/data`

5. **Push**: Push to your branch

6. **PR**: Create pull request to `main`
   - CI will run full test suite
   - Content changes will be validated
   - Links will be checked

### CI/CD Workflows

Key workflows in `.github/workflows/`:

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `test.yml` | PR, push to main | Run full test suite (30+ test modules) |
| `content-lint-markdown.yml` | PR with content changes | Validate content quality |
| `content-changes-table-comment.yml` | PR | Show changed files in comment |
| `purge-fastly.yml` | Production deploy | Purge CDN cache |
| `sync-search-indices.yml` | Daily at 2pm PST | Update Elasticsearch indices |
| `repo-sync.yml` | Multiple times daily | Sync between repos |

---

## Code Conventions

### TypeScript Configuration

**tsconfig.json**:
- Target: ES2022
- Strict mode: enabled
- Path alias: `@/` → `./src/`
- Module: ESNext (Next.js compatibility)
- Incremental builds: enabled

### Code Style (Enforced by ESLint + Prettier)

**TypeScript/TSX**:
```typescript
// ✅ Good
import { Component } from '@/frame/components/Component'

export function myFunction(param: string): boolean {
  if (param === 'value') {
    return true
  }
  return false
}

// ❌ Bad (will be auto-fixed)
export function myFunction(param: string): boolean {
  if (param === "value") {   // Double quotes (should be single)
    return true;             // Semicolons (should be removed)
  }
  return false;
}
```

**Configuration**:
- Line width: 100 characters
- Indentation: 2 spaces
- Quotes: Single quotes (except JSON)
- Semicolons: None
- Trailing commas: ES5
- Arrow function parentheses: Always

### File Naming

| Type | Convention | Example |
|------|------------|---------|
| Content files | kebab-case (matches title) | `about-github-cli.md` |
| TypeScript files | kebab-case | `process-content.ts` |
| React components | PascalCase | `ArticleCard.tsx` |
| Test files | `{name}.test.ts` | `search.test.ts` |
| SCSS files | kebab-case | `article-card.scss` |

### Import Organization

```typescript
// 1. External dependencies
import { useState } from 'react'
import cheerio from 'cheerio'

// 2. Internal dependencies (use @ alias)
import { getPathWithLanguage } from '@/languages/lib/languages'
import { ArticleCard } from '@/frame/components/ArticleCard'

// 3. Local imports
import { processMarkdown } from './lib/markdown'
```

### Component Patterns

**Prefer functional components**:
```typescript
// ✅ Good
export function ArticleCard({ title, intro }: ArticleCardProps) {
  return (
    <div>
      <h2>{title}</h2>
      <p>{intro}</p>
    </div>
  )
}

// ❌ Avoid class components unless necessary
```

**Use TypeScript types**:
```typescript
// Define types for props
type ArticleCardProps = {
  title: string
  intro: string
  href?: string
}

export function ArticleCard({ title, intro, href }: ArticleCardProps) {
  // ...
}
```

---

## Content Structure & Authoring

### Content File Anatomy

Every content file has:
1. **Frontmatter** (YAML header with metadata)
2. **Markdown body** (with Liquid templating)

**Example**:
```markdown
---
title: Understanding GitHub Actions
shortTitle: Understanding Actions
intro: Learn about the fundamentals of GitHub Actions.
versions:
  fpt: '*'
  ghes: '>=3.0'
  ghec: '*'
type: overview
topics:
  - Actions
  - Fundamentals
redirect_from:
  - /actions/learn-github-actions/introduction-to-github-actions
---

GitHub Actions is a {% data variables.product.prodname_actions %} feature...

{% data reusables.actions.about-workflows %}

## Creating your first workflow

To create a workflow...
```

### Required Frontmatter Fields

| Field | Required | Purpose | Example |
|-------|----------|---------|---------|
| `title` | Yes | Article title (H1) | `"Understanding GitHub Actions"` |
| `intro` | Yes | Short introduction | `"Learn about the fundamentals..."` |
| `versions` | Yes | Product version support | `fpt: '*'`, `ghes: '>=3.0'` |
| `type` | Recommended | Content type | `overview`, `tutorial`, `how_to`, `reference`, `quick_start` |

### Optional Frontmatter Fields

| Field | Purpose | Example |
|-------|---------|---------|
| `shortTitle` | Breadcrumb title (max 31 chars) | `"Understanding Actions"` |
| `topics` | Categorization | `["Actions", "Fundamentals"]` |
| `redirect_from` | Old URLs to redirect | `["/old/path"]` |
| `defaultPlatform` | Default OS tab | `mac`, `windows`, `linux` |
| `defaultTool` | Default tool tab | `webui`, `cli`, `desktop`, `curl` |
| `permissions` | Required permissions | `"You must be an admin..."` |

### Versioning

**Version Codes**:
- `fpt`: Free, Pro, Team (GitHub.com)
- `ghec`: Enterprise Cloud
- `ghes`: Enterprise Server (version-specific)
- `ghae`: Azure (deprecated)

**Syntax**:
```yaml
versions:
  fpt: '*'              # All versions
  ghes: '>=3.5'         # Server 3.5 and above
  ghec: '*'
```

**Conditional content**:
```markdown
{% ifversion ghes %}
This content only appears on Enterprise Server.
{% endif %}

{% ifversion fpt or ghec %}
This content appears on GitHub.com and Enterprise Cloud.
{% endif %}
```

### Liquid Templating

**Variables**:
```liquid
{% data variables.product.prodname_dotcom %}  → GitHub
{% data variables.product.prodname_actions %} → GitHub Actions
{% data variables.product.prodname_copilot %} → GitHub Copilot
```

**Reusables**:
```liquid
{% data reusables.repositories.create_new %}
{% data reusables.actions.workflow-basic-example %}
```

**Notes/Warnings**:
```liquid
{% note %}
This is a note.
{% endnote %}

{% warning %}
This is a warning.
{% endwarning %}

{% tip %}
This is a tip.
{% endtip %}
```

**Platform/Tool Switchers**:
```markdown
{% webui %}
Content for web UI
{% endwebui %}

{% cli %}
Content for CLI
{% endcli %}

{% mac %}
Content for macOS
{% endmac %}
```

### Linking

**Internal links** (use AUTOTITLE):
```markdown
For more information, see [AUTOTITLE](/actions/learn-github-actions/understanding-github-actions).
```

**External links**:
```markdown
For more information, see the [Node.js documentation](https://nodejs.org/docs).
```

### File Naming Convention

Article filename MUST match title in kebab-case:

| Title | Filename |
|-------|----------|
| "Understanding GitHub Actions" | `understanding-github-actions.md` |
| "About GitHub CLI" | `about-github-cli.md` |
| "Creating a pull request" | `creating-a-pull-request.md` |

### Content Style Guidelines

**Voice & Tone**:
- ✅ Second person ("you")
- ✅ Active voice
- ✅ Present tense
- ❌ First person ("we", "I")

**Examples**:
```markdown
✅ You can create a workflow by...
❌ We can create a workflow by...

✅ Click **Actions**
❌ The user should click Actions

✅ GitHub Actions runs your workflow
❌ Your workflow will be run by GitHub Actions
```

**Inclusive Language**:
- ✅ allowlist / denylist
- ❌ whitelist / blacklist
- ✅ primary / secondary
- ❌ master / slave

For complete style guide: https://docs.github.com/en/contributing/style-guide-and-content-model/style-guide

---

## Testing

### Test Organization

Tests follow the subject folder pattern:
```
src/{subject}/tests/
  ├── unit.test.ts        # Unit tests
  ├── integration.test.ts # Integration tests
  └── fixtures/           # Test data
```

### Running Tests

```bash
# All tests
npm test

# Specific test file
npm test -- src/search/tests/search.test.ts

# Watch mode
npm test -- --watch

# Coverage
npm test -- --coverage

# E2E tests
npm run playwright-test
```

### Writing Tests

**Vitest example**:
```typescript
import { describe, it, expect } from 'vitest'
import { processMarkdown } from './markdown'

describe('processMarkdown', () => {
  it('converts markdown to HTML', () => {
    const input = '# Hello'
    const output = processMarkdown(input)
    expect(output).toContain('<h1>Hello</h1>')
  })

  it('handles Liquid variables', () => {
    const input = '{{ site.data.variables.product }}'
    const output = processMarkdown(input)
    expect(output).toContain('GitHub')
  })
})
```

### Content Linting

**Run content linter**:
```bash
# Lint all content
npm run lint-content

# Lint specific files
npm run lint-content -- --paths content/actions/index.md

# Fix auto-fixable issues
npm run lint-content -- --fix
```

**Common content lint rules**:
- Frontmatter schema validation
- Image alt text required
- Link format validation
- Liquid syntax validation
- Hardcoded product names (should use variables)

---

## Build & Deployment

### Local Build

```bash
# Development build
npm run build

# Production build
NODE_ENV=production npm run build

# Run production build
npm run start-for-production
```

### Docker Build

Multi-stage Dockerfile:
```bash
# Build image
docker build -t github-docs .

# Run container
docker run -p 4000:4000 github-docs
```

**Build stages**:
1. `BASE`: Install Node.js 24.x
2. `CLONES`: Fetch docs-internal, early-access, translations
3. `PROD_DEPS`: Install production dependencies
4. `ALL_DEPS`: Add dev dependencies
5. `BUILD`: Run Next.js build
6. `FINAL`: Production image with only runtime deps

### Environment Variables

**Development** (`.env`):
```bash
# Search
ELASTICSEARCH_URL=http://localhost:9200

# Language selection
ENABLED_LANGUAGES=en,ja,pt

# Content root
ROOT=.

# Node environment
NODE_ENV=development
```

**Production**:
```bash
NODE_ENV=production
ELASTICSEARCH_URL=https://prod-elasticsearch:9200
FASTLY_SERVICE_ID=xxx
FASTLY_TOKEN=xxx
```

### Deployment Process

1. **Merge to main**: Changes merged to main branch
2. **CI runs**: Full test suite executes
3. **Build**: Docker image built
4. **Deploy**: Pushed to Moda platform
5. **Search index**: Updated every 24 hours via cron
6. **CDN purge**: Fastly cache purged for changed content

---

## Contributing Guidelines

### What's Accepted

✅ **Content contributions**:
- Technical/grammatical corrections
- Typo fixes
- Expanded explanations (with compelling justification)
- New articles filling documentation gaps
- Updates to `/content/*.md`
- Updates to `/data/reusables/*.md` and `/data/variables/*.yml`

❌ **Not accepted** (in public repo):
- Infrastructure changes (workflows, CI/CD)
- Build process modifications
- Dependency updates (handled by GitHub team)
- Pure style/tone changes without technical merit
- Niche/personal preference topics

### Process for External Contributors

1. **Small changes**: Use "Make a contribution" button on docs.github.com
   - Automatically creates fork and branch
   - Opens PR with changes

2. **Larger changes**:
   ```bash
   # Fork repo on GitHub
   git clone https://github.com/YOUR_USERNAME/docs
   cd docs
   git checkout -b my-feature

   # Make changes
   npm ci
   npm start  # Test locally

   # Commit and push
   git add .
   git commit -m "Description of changes"
   git push origin my-feature

   # Open PR on GitHub
   ```

3. **PR review**:
   - Automated checks run (tests, linting, content validation)
   - Docs team reviews
   - Feedback addressed
   - Merged by maintainer

### Pre-commit Hooks

Husky + lint-staged auto-run:
```json
{
  "*.{ts,tsx}": "eslint --cache --fix",
  "*.{scss,ts,tsx,yml,yaml}": "prettier --write",
  "{content,data}/**/*.md": "npm run lint-content -- --precommit --paths"
}
```

**What this means**:
- TypeScript files are linted on commit
- Code is auto-formatted with Prettier
- Content files are validated
- Commits will fail if linting errors can't be auto-fixed

---

## Common Tasks

### Task: Add a new article

1. **Determine location**:
   ```
   /content/{category}/{subcategory}/{article-name}.md
   ```

2. **Create file with frontmatter**:
   ```markdown
   ---
   title: Your Article Title
   intro: Brief introduction to the article.
   versions:
     fpt: '*'
     ghes: '>=3.0'
     ghec: '*'
   type: overview
   topics:
     - Category
   ---

   Your content here...
   ```

3. **Add to parent index**:
   Edit `/content/{category}/{subcategory}/index.md`:
   ```yaml
   children:
     - /article-name
   ```

4. **Test locally**:
   ```bash
   npm start
   # Visit http://localhost:4000/en/{category}/{subcategory}/{article-name}
   ```

5. **Lint**:
   ```bash
   npm run lint-content -- --paths content/{category}/{subcategory}/{article-name}.md
   ```

### Task: Update a reusable

1. **Find reusable**: `/data/reusables/{category}/{topic}.md`

2. **Edit**: Make changes to Markdown

3. **Find usage**:
   ```bash
   grep -r "reusables.{category}.{topic}" content/
   ```

4. **Test**: Verify all pages using the reusable render correctly

### Task: Add a product variable

1. **Edit**: `/data/variables/product.yml`

2. **Add entry**:
   ```yaml
   prodname_your_feature: GitHub Your Feature
   ```

3. **Use in content**:
   ```liquid
   {% data variables.product.prodname_your_feature %}
   ```

### Task: Fix a TypeScript component

1. **Locate component**: `/src/frame/components/{Component}.tsx`

2. **Make changes**: Follow TypeScript conventions

3. **Test**:
   ```bash
   npm test -- src/frame/tests/
   npm run lint
   ```

4. **Build**:
   ```bash
   npm run build
   npm start  # Verify in browser
   ```

### Task: Update search index

1. **Edit**: `/src/search/scripts/index-elasticsearch.ts` (if logic changes)

2. **Run locally**:
   ```bash
   # Start Elasticsearch locally (Docker)
   docker run -p 9200:9200 -e "discovery.type=single-node" elasticsearch:8.13.1

   # Index
   npm run index-elasticsearch
   ```

3. **Test**:
   ```bash
   npm start
   # Try searches at http://localhost:4000
   ```

4. **Production**: Indices update automatically every 24 hours via GitHub Actions

### Task: Add a new API endpoint

1. **Create endpoint**: `/src/app/api/your-endpoint/route.ts`
   ```typescript
   import { NextRequest, NextResponse } from 'next/server'

   export async function GET(request: NextRequest) {
     const data = { message: 'Hello' }
     return NextResponse.json(data)
   }
   ```

2. **Add middleware** (if needed): `/src/your-feature/middleware/`

3. **Add tests**: `/src/your-feature/tests/api.test.ts`

4. **Test**:
   ```bash
   npm start
   curl http://localhost:4000/api/your-endpoint
   ```

---

## Key Files Reference

### Configuration Files

| File | Purpose | Key Settings |
|------|---------|--------------|
| `package.json` | Dependencies, scripts | Node 22/24, React 18, Next.js 15 |
| `tsconfig.json` | TypeScript config | ES2022, strict mode, path alias `@/` |
| `next.config.ts` | Next.js config | Rewrites, redirects, i18n |
| `eslint.config.ts` | Linting rules | TypeScript ESLint, Primer, a11y |
| `vitest.config.ts` | Test config | Test environment, coverage |
| `.editorconfig` | Editor settings | 2 spaces, UTF-8, LF |
| `.env.example` | Env var template | Elasticsearch URL, languages |
| `Dockerfile` | Container build | Multi-stage build, Node 24 |

### Entry Points

| File | Purpose |
|------|---------|
| `src/frame/server.ts` | Express server entry point |
| `src/app/layout.tsx` | Next.js root layout |
| `src/app/page.tsx` | Homepage |
| `src/app/[locale]/[[...path]]/page.tsx` | Dynamic content pages |

### Key Libraries

| File | Exports | Purpose |
|------|---------|---------|
| `src/content-render/index.ts` | `renderContent()` | Markdown → HTML pipeline |
| `src/languages/lib/languages.ts` | `getLanguage()` | i18n utilities |
| `src/frame/lib/get-tree.ts` | `getTree()` | Build content tree |
| `src/search/lib/search.ts` | `search()` | Elasticsearch queries |
| `src/rest/lib/index.ts` | REST utilities | REST API docs |
| `src/graphql/lib/index.ts` | GraphQL utilities | GraphQL docs |

### Documentation

| File | Purpose |
|------|---------|
| `README.md` | Getting started guide |
| `.github/CONTRIBUTING.md` | External contributor guide |
| `contributing/content-style-guide.md` | Content writing guide |
| `contributing/content-markup-reference.md` | Liquid/Markdown syntax |
| `contributing/content-templates.md` | Content templates |
| `contributing/images-and-versioning.md` | Asset guidelines |

---

## Additional Resources

### Official Documentation
- **Contributing guide**: https://docs.github.com/en/contributing
- **Style guide**: https://docs.github.com/en/contributing/style-guide-and-content-model/style-guide
- **Markup reference**: https://docs.github.com/en/contributing/syntax-and-versioning-for-github-docs
- **Content model**: https://docs.github.com/en/contributing/style-guide-and-content-model/contents-of-a-github-docs-article

### Internal Documentation
- `/contributing/` directory: 20+ in-depth guides
- Subject READMEs: `/src/{subject}/README.md`
- Workflow comments: `.github/workflows/*.yml`

### Helpful Commands

```bash
# Find content
find content -name "*actions*.md"

# Search content
grep -r "GitHub Actions" content/

# Find component usage
grep -r "import.*ArticleCard" src/

# Check translations
ls translations/

# View recent changes
git log --oneline --graph --decorate -20

# Check branch sync status
git fetch origin
git log --oneline origin/main ^main
```

---

## Tips for AI Assistants

### Do's ✅

- **Read before editing**: Always use Read tool before Edit/Write
- **Test locally**: Run `npm start` and verify changes in browser
- **Lint content**: Run `npm run lint-content` after content changes
- **Check versions**: Ensure content works for all target versions
- **Follow naming**: Match file names to article titles (kebab-case)
- **Use variables**: Use Liquid variables for product names
- **Provide context**: Explain why changes improve docs
- **Keep it simple**: Prefer clarity over cleverness
- **Respect scope**: Only edit content/data in public repo

### Don'ts ❌

- **Don't guess**: If unsure, search the codebase or ask
- **Don't break versioning**: Test conditional content thoroughly
- **Don't hardcode**: Use variables and reusables
- **Don't skip tests**: Always run tests before committing
- **Don't modify infrastructure**: Workflows/CI not accepted in public repo
- **Don't ignore style**: Follow the style guide strictly
- **Don't create unnecessary files**: Edit existing files when possible
- **Don't commit secrets**: Never commit API keys, tokens, passwords

### When in Doubt

1. **Check existing patterns**: Find similar content/code and follow that pattern
2. **Read the docs**: Check `/contributing/` directory
3. **Test locally**: `npm start` and verify in browser
4. **Ask for help**: Create a discussion or issue if unclear

---

## Changelog

| Date | Changes |
|------|---------|
| 2025-11-18 | Initial creation - comprehensive repository analysis and documentation |

---

**Maintained by**: AI-assisted documentation
**For updates**: Submit PR with changes to this file
**Questions**: See `.github/CONTRIBUTING.md` or https://docs.github.com/en/contributing
