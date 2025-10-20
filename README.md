# Nordic Unicorn News 🫎

**Tracking tomorrow's unicorns across the frozen north**

A real-time intelligence platform covering Nordic tech IPOs, listings, and innovation - with a focus on Defence, AI, and Mining sectors.

## Project Status

**Phase:** Development (Foundation MVP)
**Progress:** 5% - Initial setup complete

See [STATUS.md](./STATUS.md) for detailed progress tracking.

## Quick Start

**For AI Assistants:** Read [AGENTS.md](./AGENTS.md) for complete development guidelines

**For Developers:**
```bash
# Install dependencies (once configured)
npm install

# Start development server (once configured)
npm run dev

# Run tests (once configured)
npm test
```

## Documentation

- **[AGENTS.md](./AGENTS.md)** - AI assistant guidelines and development standards
- **[STATUS.md](./STATUS.md)** - Project status and progress tracking
- **[NEXT-SESSION-START-HERE.md](./NEXT-SESSION-START-HERE.md)** - Quick session context
- **[nordic-unicorn-readme.md](./nordic-unicorn-readme.md)** - Complete project vision

## Architecture

```
nordic-unicorn-news/
├── src/
│   ├── server/              # Express backend (API)
│   ├── frontend/            # Svelte PWA
│   └── shared/              # Shared types/utils
├── intelligence/            # Private scraping & agents (not deployed)
├── docker/                  # Docker configs
└── docs/                    # Documentation
```

## Tech Stack

- **Frontend:** Svelte + SvelteKit + Tailwind CSS
- **Backend:** Express.js (REST API)
- **Database:** PostgreSQL + Qdrant (vector search)
- **Deployment:** Docker on Beast (home lab) via Cloudflare Tunnel

## Development Workflow

This project follows **Jimmy's Workflow** (Red/Green/Blue checkpoints):
- 🔴 **RED**: Implement (build, code, deploy)
- 🟢 **GREEN**: Validate (prove it works)
- 🔵 **CHECKPOINT**: Gate (lock in progress, document rollback)

See [JIMMYS-WORKFLOW.md](./JIMMYS-WORKFLOW.md) for complete system.

## License

MIT

---

**Built with 🫎 in Sweden**

This project follows the [agents.md](https://agents.md/) standard for AI coding assistants.
