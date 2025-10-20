# Nordic Unicorn News 🫎

**Tracking tomorrow's unicorns across the frozen north**

A real-time intelligence platform covering Nordic tech IPOs, listings, and innovation - with a focus on Defence, AI, and Mining sectors.

---

## Project Vision

Nordic Unicorn News is a two-layered system:

**Public Layer:** A free, accessible news site providing English-language coverage of Nordic IPOs, First North listings, and tech innovation. Think Bloomberg meets Scandinavian minimalism, with a moose mascot.

**Private Intelligence Layer:** A proprietary trading intelligence system that aggregates, analyzes, and feeds market data to autonomous trading agents. This layer remains invisible to the public.

---

## Core Sectors

We focus on three high-growth, underreported Nordic sectors:

- **Defence** - Nordic defence tech, NATO expansion beneficiaries, surveillance systems
- **AI** - Applied AI in enterprise, infrastructure, model training companies
- **Mining Services** - AI-powered exploration, automation, predictive maintenance

---

## Architecture

### Monorepo Structure
```
nordic-unicorn-news/
├── src/
│   ├── server/              # Express backend (API)
│   ├── frontend/            # Svelte PWA
│   └── shared/              # Shared types/utils
├── intelligence/            # Private scraping & agents (not deployed)
├── Dockerfile
├── docker-compose.yml
└── README.md
```

### Tech Stack

**Public Site (Single Container):**
- **Frontend:** Svelte + SvelteKit + Tailwind CSS
- **Backend:** Express.js (REST API)
- **Database:** PostgreSQL (structured data) + Qdrant (vector search)
- **Deployment:** Docker container, Cloudflare Tunnel, home lab hosting

**Intelligence Pipeline (Separate):**
- **Event Streaming:** Kafka
- **Scrapers:** Python/Rust (Nasdaq OMX, First North, Breakit, DI, FSA filings)
- **AI Processing:** Claude API for analysis, translation, summarization
- **RAG System:** Qdrant vector database
- **Trading Integration:** Nordnet API for autonomous execution

### Data Flow

```
Scrapers → Kafka → AI Analysis → Qdrant (RAG)
                         ↓
              Trading Agents (private)
                         ↓
              Public DB ← API ← Frontend (PWA)
```

---

## Key Features

### Public Site
- Real-time IPO and listing announcements
- Company profiles (basic auto-generated, enhanced for key companies)
- Personalized newsletters (sector-based subscriptions)
- Company watchlists (tracks user interest)
- English translations of Swedish/Norwegian/Danish sources
- Progressive Web App (installable, offline-capable)
- Moose-with-unicorn-horn branding 🫎

### Intelligence Layer (Private)
- Multi-source news aggregation and correlation
- AI-powered prospectus analysis
- Valuation modeling and comparable company analysis
- Sentiment tracking and pattern recognition
- Real-time alerts based on custom criteria
- Historical performance tracking
- Integration with autonomous trading agents

---

## Brand Identity

**Logo:** Nordic moose with unicorn horn, surrounded by Celtic knotwork, northern lights, and runic circuit patterns

**Tone:** Serious financial intelligence delivered with Nordic charm. We don't take ourselves too seriously, but we take the data very seriously.

**Color Palette:**
- Deep blues and teals (Nordic waters, northern lights)
- Gold accents (premium, trustworthy)
- Dark mode default (professional, easy on eyes)

**Tagline Options:**
- "Following the herds to find the unicorns"
- "Where Nordic legends meet billion-dollar futures"
- "Defence, AI, Mining - The Nordic way"

---

## Data Model

### Companies
```
company_id, name, ticker, market, sector, country, 
listing_date, market_cap, profile_type (basic|enhanced),
prospectus_url, website, created_at, updated_at
```

### Articles
```
article_id, company_id, title, summary, content, 
source, source_url, published_at, sector_tags,
ai_generated, language, created_at
```

### Users (Newsletter)
```
user_id, email, sectors[], stage_preference[], 
geography[], frequency, verified, created_at
```

### Watchlists
```
user_id, company_id, added_at
```

### Engagement Tracking
```
user_id, article_id, company_id, event_type,
timestamp, metadata
```

---

## Newsletter System

**Personalization:**
- Users select sectors (Defence, AI, Mining, All)
- Choose company stages (Pre-IPO, Recently Listed, Growth)
- Pick geography (Sweden, Norway, Denmark, Finland, All Nordic)
- Set frequency (Daily, Weekly, Breaking Only)

**Intelligence Gathering:**
- Track open rates by sector
- Monitor which companies get the most watches
- Identify trending topics before they hit mainstream
- Feed engagement signals back to trading models

---

## Development Priorities

### Phase 1: Foundation (MVP)
- Single container deployment (Express + Svelte)
- Basic company profiles (auto-generated)
- Simple article publishing system
- Newsletter signup with sector selection
- Responsive design with Tailwind

### Phase 2: Intelligence
- Connect to existing Kafka pipeline
- Auto-translation of Nordic sources
- AI-powered article generation
- Enhanced company profiles for key sectors
- Watchlist functionality

### Phase 3: Community
- User accounts and preferences
- Personalized newsletter delivery
- Engagement analytics dashboard
- Comment system or community features
- Social sharing optimization

### Phase 4: Intelligence Feedback Loop
- Sentiment scoring based on user behavior
- Crowd interest signals for trading agents
- Pattern recognition on watchlist activity
- Predictive models using engagement data

---

## Infrastructure

**Hosting:**
- Home lab: AIx (96GB RAM, Ryzen AI 9 HX 370, 1Gbps fiber)
- Cloudflare Tunnel for HTTPS and caching
- Docker containerization
- Dual 2.5G ethernet

**Capacity:**
- Can handle 100K+ daily visitors
- 1000+ concurrent users
- Millions of articles
- Real-time updates

**Cost:**
- $0 hosting (home lab)
- ~$50-200/month APIs (Claude, translation, data sources)
- Cloudflare Free tier

---

## Success Metrics

**Public Site:**
- Newsletter subscribers by sector
- Daily active users
- Article engagement rates
- Company watchlist growth
- Social shares and backlinks

**Intelligence Value (Private):**
- Hours of information advantage vs. international press
- Accuracy of sentiment signals
- Trading alpha generated
- Pattern recognition success rate

---

## Competitive Advantage

**Why We Win:**

1. **Language Barrier Breaking** - First comprehensive English coverage of Nordic IPO market
2. **Sector Focus** - Deep expertise in Defence/AI/Mining vs. generic tech coverage
3. **Real-time Intelligence** - AI-powered aggregation beats manual curation
4. **Free Access** - Democratizing information that's currently behind paywalls or language barriers
5. **Community Signals** - User behavior provides trading intelligence
6. **Technical Infrastructure** - Modern stack, scalable, fast

**Moat:**
- Historical database of Nordic listings (compound over time)
- Established newsletter audience with known preferences
- AI models trained on Nordic financial language
- Integration with trading systems (proprietary advantage)

---

## Getting Started

```bash
# Install dependencies
npm install

# Run development servers
npm run dev

# Build for production
npm run build

# Deploy container
docker-compose up -d
```

---

## Legal & Ethics

- All content properly attributed to sources
- No reproduction of copyrighted material
- Clear disclosure of AI-generated content
- Privacy-first approach to user data
- Transparent about data collection for newsletters
- No misleading investment advice

---

## Future Possibilities

- Native mobile apps (if user demand warrants)
- API access for developers (freemium model)
- Premium features (advanced analytics, API access, real-time alerts)
- Partnerships with Nordic VCs and exchanges
- Expansion to other emerging European markets
- Affiliate revenue from Nordic broker signups

---

**Built with 🫎 in Sweden**

For questions or collaboration: [contact info]

---

## Notes for AI Coding Assistants

This project uses:
- Svelte for frontend (not React)
- Tailwind for styling (utility-first CSS)
- Express for backend API
- PostgreSQL + Qdrant for data
- Monorepo structure (shared types between frontend/backend)
- Single container deployment
- PWA requirements (service worker, manifest.json)
- Nordic aesthetic (blues, teals, gold, minimalist)
- Mobile-first responsive design
- Dark mode default

The intelligence pipeline (Kafka, scrapers, agents) is separate and not part of the public site deployment.