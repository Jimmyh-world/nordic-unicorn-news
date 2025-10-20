# Nordic Unicorn News - Nordic Tech IPO Intelligence Platform

<!--
TEMPLATE_VERSION: 1.5.0
TEMPLATE_SOURCE: /home/jimmyb/templates/AGENTS.md.template
LAST_SYNC: 2025-10-20
SYNC_CHECK: Run ~/templates/tools/check-version.sh to verify you have the latest template version
AUTO_SYNC: Run ~/templates/tools/sync-templates.sh to update (preserves your customizations)
CHANGELOG: See ~/templates/CHANGELOG.md for version history
-->

**STATUS: IN DEVELOPMENT** - Last Updated: 2025-10-20

## Repository Information
- **GitHub Repository**: https://github.com/Jimmyh-world/nordic-unicorn-news (to be created)
- **Local Directory**: `/home/jimmyb/unicorn-news`
- **Primary Purpose**: Real-time intelligence platform covering Nordic tech IPOs, listings, and innovation in Defence, AI, and Mining sectors

## Important Context

<!-- PROJECT_SPECIFIC START: IMPORTANT_CONTEXT -->
**Nordic Unicorn News** is a two-layered intelligence platform:

1. **Public Layer**: Free, English-language news site covering Nordic IPOs and First North listings. Think Bloomberg meets Scandinavian minimalism, with a moose-with-unicorn-horn mascot.

2. **Private Intelligence Layer**: Proprietary trading intelligence system that aggregates, analyzes, and feeds market data to autonomous trading agents (this layer remains invisible to the public).

**Why This Exists**: Nordic markets are underreported in English, creating information advantages for those who can aggregate Swedish/Norwegian/Danish sources quickly. We break the language barrier while building community interest signals for trading intelligence.

**Fun Factor**: Serious financial intelligence delivered with Nordic charm. We don't take ourselves too seriously, but we take the data very seriously. Following the herds to find the unicorns! 🫎
<!-- PROJECT_SPECIFIC END: IMPORTANT_CONTEXT -->

## Core Development Principles (MANDATORY)

### 1. KISS (Keep It Simple, Stupid)
- Avoid over-complication and over-engineering
- Choose simple solutions over complex ones
- Question every abstraction layer
- If a feature seems complex, ask: "Is there a simpler way?"

### 2. TDD (Test-Driven Development)
- Write tests first
- Run tests to ensure they fail (Red phase)
- Write minimal code to pass tests (Green phase)
- Refactor while keeping tests green
- Never commit code without tests

### 3. Separation of Concerns (SOC)
- Each module/component has a single, well-defined responsibility
- Clear boundaries between different parts of the system
- Services should be loosely coupled
- Avoid mixing business logic with UI or data access code

### 4. DRY (Don't Repeat Yourself)
- Eliminate code duplication
- Extract common functionality into reusable components
- Use configuration files for repeated settings
- Create shared libraries for common operations

### 5. Documentation Standards
- Always include the actual date when writing documentation
- Use objective, factual language only
- Avoid marketing terms like "production-ready", "world-class", "highly sophisticated", "cutting-edge", etc.
- State current development status clearly
- Document what IS, not what WILL BE

### 5.5. AI-Optimized Documentation
**CRITICAL**: Documentation is structured data for both humans AND AI consumption

**Purpose**: Enable AI assistants to effectively help during:
- **Development** (now) - Building the system
- **Deployment** (later) - Setting up and configuring on Beast
- **Operations** (ongoing) - Monitoring, troubleshooting
- **User Support** (ongoing) - Helping users use the system

**Key Principles**:
1. **Structured Data Over Prose** - Use tables, JSON, YAML instead of paragraphs
2. **Explicit Context** - Never assume prior knowledge
3. **Cause-Effect Relationships** - Clear "if X then Y" statements
4. **Machine-Readable Examples** - Complete, runnable code blocks
5. **Searchable Patterns** - Consistent headings, markers, formats
6. **Version-Stamped** - Date all documentation updates
7. **Cross-Referenced** - Explicit links between related docs

**Example** (Good AI-optimized documentation):
```markdown
## Database Configuration

**Required Environment Variables**:
| Variable | Format | Example | Required |
|----------|--------|---------|----------|
| DATABASE_URL | postgresql://... | postgresql://postgres:secret@localhost:5432/db | Yes |

**Validation**:
\```bash
npm run db:test-connection
# Expected output: "✅ Connected successfully"
\```
```

**Documentation Layers**:
- **Layer 1**: Development Phase (AGENTS.md, Architecture docs, Phase plans)
- **Layer 2**: Deployment Phase (DEPLOYMENT-GUIDE.md, OPERATIONS.md, API-REFERENCE.md)
- **Layer 3**: User Phase (USER-GUIDE.md, TROUBLESHOOTING.md, Configuration patterns)

All documentation follows these principles to maximize AI assistant effectiveness.

### 6. Jimmy's Workflow (Red/Green Checkpoints)
**MANDATORY for all implementation tasks**

Use the Red/Green/Blue checkpoint system to prevent AI hallucination and ensure robust implementation:

- 🔴 **RED (IMPLEMENT)**: Write code, build features, make changes
- 🟢 **GREEN (VALIDATE)**: Run explicit validation commands, prove it works
- 🔵 **CHECKPOINT**: Mark completion with machine-readable status, document rollback

**Critical Rules:**
- NEVER skip validation phases
- NEVER proceed to next checkpoint without GREEN passing
- ALWAYS document rollback procedures
- ALWAYS use explicit validation commands (not assumptions)

**Reference**: See **JIMMYS-WORKFLOW.md** for complete workflow system, templates, and patterns

**Usage**: When working with AI assistants, say: *"Let's use Jimmy's Workflow to execute this plan"*

**Benefits:**
- Prevents "AI says done ≠ Actually done" problem
- Forces validation at every step
- Enables autonomous execution with safety gates
- Provides clear rollback paths
- Integrates seamlessly with TDD approach

### 7. YAGNI (You Ain't Gonna Need It)
- Don't implement features until they're actually needed
- Resist the urge to "future-proof" or add "might be useful later" code
- Build for current requirements, not hypothetical future ones
- Question every feature: "Do we need this NOW?"
- Refactor when requirements change, don't pre-optimize
- Every line of code is a liability - only write what's necessary

**Why This Matters:**
- Prevents scope creep and over-engineering
- Reduces technical debt (unused code is still debt)
- Speeds up development (focus on actual requirements)
- Forces clear prioritization of features

**AI Assistant Reminder:** Don't add "helpful" features like caching, abstraction layers, or config systems unless explicitly required by current needs.

### 8. Fix Now, Not Later
- Fix vulnerabilities immediately when discovered (npm audit, security warnings)
- Fix warnings immediately (don't suppress or accumulate)
- Fix failing tests immediately (understand root cause, don't skip)
- Fix linter errors immediately (don't disable rules without reason)
- Address build errors and deprecation warnings as they appear
- Don't use suppressions (@ts-ignore, eslint-disable, etc.) without documented justification

**Exception Clause:**
- If you MUST defer or bypass an issue:
  1. Investigate the root cause thoroughly
  2. Weigh multiple solution options
  3. Make an explicit decision
  4. DOCUMENT why (in code comments, KNOWN_ISSUES.md, or technical debt tracker)
  5. Create a tracking issue/ticket

**Why This Matters:**
- Prevents technical debt accumulation
- Keeps codebase healthy and maintainable
- Vulnerabilities don't ship to production
- Warnings don't become noise
- Tests remain meaningful

**AI Assistant Reminder:** Never suggest "we'll fix this later" or "skip for now". Always investigate root cause and fix immediately. If deferring is necessary, document comprehensively.

## GitHub Workflow

### Use GitHub CLI (gh) for All GitHub Operations

**Standard Tool**: Use `gh` CLI for all GitHub interactions (issues, PRs, CI/CD monitoring, releases)

**Installation**: `gh` should already be installed. Verify with `gh --version`

**Common Operations:**

**Pull Requests:**
```bash
gh pr create --title "Feature" --body "Description"
gh pr list                          # View open PRs
gh pr checks                        # Check CI/CD status
gh pr view [number]                 # View PR details
gh pr merge [number]                # Merge PR
```

**CI/CD Monitoring:**
```bash
gh run list                         # List workflow runs
gh run view [id]                    # View run details
gh run watch                        # Watch current run (live updates)
gh workflow list                    # List workflows
```

**Issues:**
```bash
gh issue create --title "Bug" --body "Description"
gh issue list                       # View open issues
gh issue view [number]              # View issue details
gh issue close [number]             # Close issue
```

**Releases:**
```bash
gh release create v1.0.0            # Create release
gh release list                     # List releases
gh release view [tag]               # View release details
```

**Why GitHub CLI:**
- ✅ Scriptable and automation-friendly
- ✅ Consistent across all projects
- ✅ Works seamlessly with AI assistants
- ✅ Faster than web UI for most operations
- ✅ Built-in CI/CD monitoring
- ✅ Integrates with Jimmy's Workflow checkpoints

**AI Assistant Note**: Always use `gh` commands instead of suggesting "check the GitHub web UI" or manual git operations for GitHub-specific tasks.

## Service Overview

<!-- PROJECT_SPECIFIC START: SERVICE_OVERVIEW -->
**Nordic Unicorn News** is a monorepo full-stack application that provides:

1. **Public News Site** (Svelte PWA):
   - Real-time Nordic IPO and listing announcements
   - Company profiles (auto-generated basic, enhanced for key sectors)
   - Personalized newsletters by sector (Defence, AI, Mining)
   - Company watchlists
   - English translations of Nordic sources
   - Progressive Web App (installable, offline-capable)

2. **Private Intelligence Pipeline** (separate from deployment):
   - Multi-source news aggregation (Kafka-based)
   - AI-powered prospectus analysis (Claude API)
   - Sentiment tracking and pattern recognition
   - RAG system for company knowledge (Qdrant)
   - Integration with trading agents (Nordnet API)

**Key Responsibilities:**
- Aggregate Nordic IPO and listing data from multiple sources
- Translate Swedish/Norwegian/Danish content to English
- Generate company profiles and analyses using AI
- Deliver personalized newsletters based on sector preferences
- Track user engagement to feed trading intelligence
- Provide public API for Nordic startup ecosystem data

**Important Distinctions:**
- **Public vs Private**: The website is public and free. The trading intelligence layer is private and never exposed
- **Basic vs Enhanced Profiles**: All companies get auto-generated profiles. Defence/AI/Mining companies get human-reviewed enhanced profiles
- **News vs Intelligence**: News articles are public. Trading signals derived from user behavior are private
<!-- PROJECT_SPECIFIC END: SERVICE_OVERVIEW -->

## Current Status

<!-- PROJECT_SPECIFIC START: CURRENT_STATUS -->
🔄 **Initial Development** - 0% Complete

**Phase 1: Foundation (MVP)**
- ⚪ Monorepo structure (Express + Svelte)
- ⚪ Docker container configuration
- ⚪ Database schema (PostgreSQL)
- ⚪ Basic company profile model
- ⚪ Simple article publishing
- ⚪ Newsletter signup with sector selection
- ⚪ Responsive design with Tailwind

**Phase 2: Intelligence**
- ⚪ Kafka pipeline integration
- ⚪ Auto-translation (Swedish/Norwegian/Danish → English)
- ⚪ AI-powered article generation
- ⚪ Enhanced company profiles
- ⚪ Watchlist functionality

**Phase 3: Community**
- ⚪ User accounts
- ⚪ Newsletter delivery system
- ⚪ Engagement analytics
- ⚪ Comment system
- ⚪ Social sharing

**Phase 4: Intelligence Feedback**
- ⚪ Sentiment scoring
- ⚪ Crowd interest signals
- ⚪ Pattern recognition
- ⚪ Predictive models
<!-- PROJECT_SPECIFIC END: CURRENT_STATUS -->

## Technology Stack

### Full-Stack Application

**Frontend:**
- **Framework**: Svelte + SvelteKit
- **Build Tool**: Vite
- **Styling**: Tailwind CSS (Nordic aesthetic: blues, teals, gold, dark mode default)
- **State Management**: Svelte stores
- **PWA**: Service worker, manifest.json
- **Testing**: Vitest

**Backend:**
- **Runtime**: Node.js
- **Framework**: Express.js (REST API)
- **Database**: PostgreSQL (structured data)
- **Vector DB**: Qdrant (RAG/semantic search)
- **Authentication**: JWT (for user accounts in Phase 3)
- **Testing**: Jest + Supertest

**Intelligence Pipeline (Separate):**
- **Event Streaming**: Kafka
- **Scrapers**: Python (Nasdaq OMX, First North, Breakit, DI Digital, FSA filings)
- **AI Processing**: Claude API (Anthropic)
- **RAG**: Qdrant vector database
- **Trading Integration**: Nordnet API

**Infrastructure:**
- **Hosting**: Home lab (AIx hardware: 96GB RAM, Ryzen AI 9 HX 370)
- **Tunnel**: Cloudflare Tunnel (HTTPS)
- **CI/CD**: GitHub Actions
- **Monitoring**: Prometheus + Grafana (already deployed on Beast)
- **Containerization**: Docker + Docker Compose

## Build & Test Commands

### Development
```bash
# Install dependencies
npm install

# Start development servers (frontend + backend)
npm run dev

# Run tests
npm test

# Type checking
npm run typecheck

# Linting
npm run lint
```

### Production
```bash
# Build for production
npm run build

# Run production build locally
npm run preview

# Deploy to Beast
docker compose up -d
```

## Repository Structure

```
unicorn-news/
├── src/
│   ├── server/              # Express backend
│   │   ├── routes/          # API routes
│   │   ├── models/          # Database models
│   │   ├── services/        # Business logic
│   │   └── tests/           # Backend tests
│   ├── frontend/            # Svelte app
│   │   ├── routes/          # SvelteKit routes
│   │   ├── components/      # Svelte components
│   │   ├── stores/          # State management
│   │   ├── lib/             # Utilities
│   │   └── tests/           # Frontend tests
│   └── shared/              # Shared types/utils
├── intelligence/            # Private intelligence pipeline (not deployed with site)
│   ├── scrapers/            # Data collection
│   ├── processors/          # AI analysis
│   └── agents/              # Trading integration
├── public/                  # Static assets
├── docker/                  # Docker configs
├── docs/                    # Documentation
├── Dockerfile
├── docker-compose.yml
├── package.json
├── AGENTS.md               # This file
├── CLAUDE.md               # Quick reference
├── JIMMYS-WORKFLOW.md      # Workflow system
└── README.md               # User documentation
```

## Development Workflow

### Starting Work on a Task
1. Read this AGENTS.md file for context
2. Check current implementation status above
3. Review known issues and TODOs below
4. **Use Jimmy's Workflow**: Plan → Implement → Validate → Checkpoint
5. Follow TDD approach - write tests first
6. Implement minimal code to pass tests
7. Refactor while maintaining green tests

### Before Committing Code
1. Run all tests: `npm test`
2. Check types: `npm run typecheck`
3. Run linter: `npm run lint`
4. Update documentation if needed
5. Ensure no credentials are exposed
6. Verify tests are passing
7. Use Jimmy's Workflow checkpoints to validate completeness

### Documentation Updates
1. Update README.md with any API changes
2. Add inline comments for complex logic
3. Update this AGENTS.md if development approach changes
4. Document all decisions with dates
5. Keep STATUS.md current

## Known Issues & Technical Debt

<!-- PROJECT_SPECIFIC START: KNOWN_ISSUES -->
### 🔴 Critical Issues
None yet - project in initial setup

### 🟡 Important Issues
None yet - project in initial setup

### 📝 Technical Debt
None yet - project in initial setup
<!-- PROJECT_SPECIFIC END: KNOWN_ISSUES -->

## Project-Specific Guidelines

<!-- PROJECT_SPECIFIC START: PROJECT_SPECIFIC_GUIDELINES -->

### Code Style
- **Framework Preference**: Use Svelte (not React) for all frontend components
- **Naming**: PascalCase for components, camelCase for functions/variables
- **File Organization**: Group by feature, not by type
- **Import Ordering**: External deps → Internal deps → Types → Styles

### Design System
- **Color Palette**: Deep blues (#1e3a8a), teals (#0d9488), gold accents (#f59e0b)
- **Typography**: Clean, readable fonts (system fonts preferred for performance)
- **Dark Mode**: Default, with light mode toggle
- **Nordic Aesthetic**: Minimalist, spacious, clean lines
- **Branding**: Moose-unicorn mascot, playful but professional tone

### Testing Requirements
- **Coverage Target**: 80%+ for business logic
- **Test Types**: Unit tests for models/services, integration tests for API, E2E for critical flows
- **Test Data**: Anonymized real Nordic company data for realistic testing

### Security Considerations
- **API Keys**: Never expose Claude API keys, Nordnet credentials
- **User Data**: GDPR compliance for EU users (newsletter emails, preferences)
- **Public vs Private**: Strict separation between public site and intelligence pipeline
- **Rate Limiting**: Protect API endpoints from abuse
<!-- PROJECT_SPECIFIC END: PROJECT_SPECIFIC_GUIDELINES -->

## Common Patterns & Examples

<!-- PROJECT_SPECIFIC START: COMMON_PATTERNS -->
### Svelte Component Pattern
Standard component structure for Nordic Unicorn News:

```svelte
<script lang="ts">
  import type { Company } from '$lib/types';

  export let company: Company;

  // Logic here
</script>

<div class="company-card">
  <h2>{company.name}</h2>
  <p>{company.sector}</p>
</div>

<style>
  /* Scoped styles using Tailwind or custom CSS */
</style>
```

### API Route Pattern
Express routes follow consistent structure:

```typescript
// src/server/routes/companies.ts
import { Router } from 'express';
import { getCompanies, getCompanyById } from '../services/companies';

const router = Router();

router.get('/companies', async (req, res) => {
  try {
    const companies = await getCompanies(req.query);
    res.json(companies);
  } catch (error) {
    res.status(500).json({ error: 'Failed to fetch companies' });
  }
});

export default router;
```
<!-- PROJECT_SPECIFIC END: COMMON_PATTERNS -->

## Dependencies & Integration

<!-- PROJECT_SPECIFIC START: DEPENDENCIES -->
### External Services
- **Claude API (Anthropic)**: AI-powered article generation, prospectus analysis, translation
- **Qdrant Cloud**: Vector database for RAG/semantic search
- **Cloudflare Tunnel**: HTTPS access from home lab
- **Nordnet API**: Trading integration (private intelligence layer)
- **Kafka**: Event streaming for scraper data (to be deployed)

### Related Services (on Beast)
- **Prometheus/Grafana**: Already deployed for monitoring
- **Portainer**: Container management
- **Cloudflare Tunnel**: Active with kitt.agency domain
- **PostgreSQL**: To be deployed for this project
- **Qdrant**: To be deployed for this project
<!-- PROJECT_SPECIFIC END: DEPENDENCIES -->

## Environment Variables

<!-- PROJECT_SPECIFIC START: ENVIRONMENT_VARIABLES -->
```bash
# Required
DATABASE_URL=postgresql://user:password@localhost:5432/unicorn_news
CLAUDE_API_KEY=sk-ant-...
QDRANT_URL=https://...qdrant.io
QDRANT_API_KEY=...

# Optional
PORT=5000 # Default: 5000
NODE_ENV=development # Default: development
CLOUDFLARE_TUNNEL_TOKEN=... # For production deployment on Beast

# Intelligence Pipeline (Separate)
KAFKA_BROKERS=localhost:9092
NORDNET_API_KEY=... # Private trading integration
```
<!-- PROJECT_SPECIFIC END: ENVIRONMENT_VARIABLES -->

## Troubleshooting

<!-- PROJECT_SPECIFIC START: TROUBLESHOOTING -->
### Common Issues

**Issue**: Docker build fails on Beast
**Solution**: Check available resources with `docker system df`. Beast has 96GB RAM so shouldn't be an issue, but verify Docker daemon is running

**Issue**: Cloudflare Tunnel not accessible
**Solution**: Check tunnel status with `cloudflared tunnel info <tunnel-id>`. Verify DNS records are correct in Cloudflare dashboard

**Issue**: Svelte HMR not working in Docker
**Solution**: Ensure Vite server is configured with `server.watch.usePolling: true` for Docker environments
<!-- PROJECT_SPECIFIC END: TROUBLESHOOTING -->

## Resources & References

### Documentation
- [Project Vision](./nordic-unicorn-readme.md) - Complete vision and architecture
- [Beast Infrastructure](../dev-network/README.md) - Deployment environment
- [Jimmy's Templates](../templates/README.md) - Project template system

### External Resources
- [SvelteKit Documentation](https://kit.svelte.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Claude API](https://docs.anthropic.com/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)

### Nordic Market Resources
- [Nasdaq OMX Nordic](https://www.nasdaqomxnordic.com/)
- [First North](https://www.nasdaq.com/solutions/first-north)
- [Breakit](https://www.breakit.se/) - Swedish tech news
- [DI Digital](https://www.di.se/digital/) - Dagens Industri tech section
- [FSA Sweden](https://www.fi.se/) - Financial Supervisory Authority

## Template Version Management

**Current Template Version**: See `<!-- TEMPLATE_VERSION -->` comment at top of this file

**This project uses versioned templates** from `/home/jimmyb/templates/`

### Check if Templates are Up to Date

```bash
~/templates/tools/check-version.sh
```

**What it does:**
- Compares your AGENTS.md version with master template version
- Exit code 0 = up to date ✅
- Exit code 1 = out of date ⚠️

### View Template Changelog

```bash
cat ~/templates/CHANGELOG.md
```

See what's new in each version and migration instructions.

### Sync to Latest Version (Manual for now)

```bash
~/templates/tools/sync-templates.sh --dry-run   # Preview changes
~/templates/tools/sync-templates.sh             # Apply changes (with confirmation)
~/templates/tools/sync-templates.sh --auto      # Auto-apply without confirmation
```

**What gets preserved during sync:**
- ✅ All `<!-- PROJECT_SPECIFIC -->` sections (your customizations)
- ✅ All placeholder values (PROJECT_NAME, commands, etc.)
- ✅ Custom additions to Known Issues, Technical Debt, etc.
- ✅ Project-specific guidelines and patterns

**What gets updated during sync:**
- 🔄 Core Development Principles (if new ones added)
- 🔄 Template structure improvements
- 🔄 Standard sections and formatting
- 🔄 Tool integrations (GitHub CLI, etc.)

**Important Notes:**
- Always review the diff before applying
- Backups are created automatically in `.template-sync-backup/`
- If sync fails, restore from backup
- Commit template updates separately from feature work

### Template Compliance Checking

**AI Assistant Behavior**: When user asks "check templates" or "are we up to date?", automatically:
1. Run: `~/templates/tools/audit-project.sh` OR manually execute checks
2. Generate compliance report
3. Offer remediation based on findings

**User can also run manually:**
```bash
~/templates/tools/audit-project.sh          # Full audit
~/templates/tools/audit-project.sh --quick  # Quick check
```

**Quick Manual Check:**
```bash
# Are we up to date?
~/templates/tools/check-version.sh

# What's new?
cat ~/templates/CHANGELOG.md
```

## Important Reminders for AI Assistants

1. **Always use Jimmy's Workflow** for implementation tasks
2. **Follow TDD** - Write tests before implementation
3. **Keep it KISS** - Simplicity over complexity
4. **Apply YAGNI** - Only implement what's needed now, not future "might need" features
5. **Use GitHub CLI** - Use `gh` for all GitHub operations (PRs, issues, CI/CD monitoring)
6. **Fix Now** - Never defer fixes for vulnerabilities, warnings, or test failures. No suppressions without documented justification
7. **Document dates** - Include actual dates in all documentation
8. **Validate explicitly** - Run commands, don't assume
9. **Never skip checkpoints** - Each phase must complete before proceeding
10. **Update this file** - Keep AGENTS.md current as project evolves
11. **Nordic Branding** - Remember the moose-unicorn mascot and playful-but-serious tone 🫎

---

**This document follows the [agents.md](https://agents.md/) standard for AI coding assistants.**

**Template Version**: 1.5.0
**Last Updated**: 2025-10-20
