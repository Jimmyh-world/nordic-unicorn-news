# Phase 2: Full-Stack Application - Execution Specification

**Created:** 2025-10-20
**For:** Beast execution
**Orchestrator:** Chromebook
**Status:** Ready for execution

---

## 🎯 Objective

Build and deploy the full-stack Nordic Unicorn News application (Svelte + Express + PostgreSQL) to replace the Coming Soon page.

**Success Criteria:**
- Users can browse Nordic IPO companies
- Users can read news articles
- Articles are fetched from ydun-scraper (https://scrape.kitt.agency)
- Newsletter signup works and stores to database
- Site deployed at https://nordicunicornnews.com

---

## 📋 Jimmy's Workflow Approach

**Every step follows RED → GREEN → CHECKPOINT:**

### 🔴 RED (IMPLEMENT)
- Write code
- Create database schemas
- Build API endpoints
- Build UI components

### 🟢 GREEN (VALIDATE)
- Run explicit validation commands
- Test database connections
- Test API endpoints with curl
- Test UI in browser
- Prove it works with real data

### 🔵 CHECKPOINT
- Mark completion
- Document rollback procedures
- Commit to git
- Update status documentation

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│  User Browser                                           │
│  https://nordicunicornnews.com                          │
└────────────────┬────────────────────────────────────────┘
                 │
                 │ HTTPS (Cloudflare Tunnel)
                 ↓
┌─────────────────────────────────────────────────────────┐
│  Beast (192.168.68.100)                                 │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Docker Compose Stack                           │   │
│  │  ┌──────────────────┐  ┌──────────────────┐    │   │
│  │  │ Frontend (Svelte)│  │ Backend (Express)│    │   │
│  │  │ Port: 5173       │  │ Port: 3002       │    │   │
│  │  │ Dev: Vite HMR    │  │ API: /api/*      │    │   │
│  │  └────────┬─────────┘  └────────┬─────────┘    │   │
│  │           │                      │              │   │
│  │           │         ┌────────────┴──────┐       │   │
│  │           │         │                   │       │   │
│  │           │    ┌────▼─────┐      ┌─────▼────┐  │   │
│  │           │    │PostgreSQL│      │ ydun-    │  │   │
│  │           │    │Port: 5432│      │ scraper  │  │   │
│  │           │    │DB: unicorn│     │(existing)│  │   │
│  │           │    └──────────┘      └──────────┘  │   │
│  │           │                                     │   │
│  │  ┌────────▼─────────────────────────────┐      │   │
│  │  │ nginx reverse proxy                  │      │   │
│  │  │ Port 3001 → routes to frontend/API   │      │   │
│  │  └──────────────────────────────────────┘      │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 📊 Database Schema Design

### Tables

#### 1. `companies`
```sql
CREATE TABLE companies (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    ticker VARCHAR(50),
    market VARCHAR(100) NOT NULL,  -- 'Nasdaq OMX Stockholm', 'First North', etc.
    sector VARCHAR(100) NOT NULL,  -- 'Defence', 'AI', 'Mining'
    country VARCHAR(50) NOT NULL,  -- 'Sweden', 'Norway', 'Denmark', 'Finland'
    listing_date DATE,
    market_cap BIGINT,
    profile_type VARCHAR(20) DEFAULT 'basic',  -- 'basic' or 'enhanced'
    description TEXT,
    website VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_companies_sector ON companies(sector);
CREATE INDEX idx_companies_country ON companies(country);
CREATE INDEX idx_companies_market ON companies(market);
```

#### 2. `articles`
```sql
CREATE TABLE articles (
    id SERIAL PRIMARY KEY,
    company_id INTEGER REFERENCES companies(id) ON DELETE CASCADE,
    title VARCHAR(500) NOT NULL,
    summary TEXT,
    content TEXT NOT NULL,
    source VARCHAR(255) NOT NULL,  -- 'Breakit', 'DI Digital', etc.
    source_url TEXT NOT NULL,
    published_at TIMESTAMP NOT NULL,
    ai_generated BOOLEAN DEFAULT FALSE,
    language VARCHAR(10) DEFAULT 'en',  -- 'en', 'sv', 'no', 'da'
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_articles_company ON articles(company_id);
CREATE INDEX idx_articles_published ON articles(published_at DESC);
CREATE INDEX idx_articles_source ON articles(source);
```

#### 3. `users` (newsletter subscribers)
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    sectors TEXT[] DEFAULT '{}',  -- ['Defence', 'AI', 'Mining']
    stage_preference TEXT[] DEFAULT '{}',  -- ['Pre-IPO', 'Recently Listed', 'Growth']
    geography TEXT[] DEFAULT '{}',  -- ['Sweden', 'Norway', 'Denmark', 'Finland']
    frequency VARCHAR(20) DEFAULT 'weekly',  -- 'daily', 'weekly', 'breaking'
    verified BOOLEAN DEFAULT FALSE,
    verification_token VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_verified ON users(verified);
```

#### 4. `watchlists`
```sql
CREATE TABLE watchlists (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    company_id INTEGER REFERENCES companies(id) ON DELETE CASCADE,
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, company_id)
);

CREATE INDEX idx_watchlists_user ON watchlists(user_id);
CREATE INDEX idx_watchlists_company ON watchlists(company_id);
```

---

## 🔧 Implementation Steps (For Beast)

### Step 1: Set Up Local Development Environment (30 min)

**🔴 RED: Install Dependencies**
```bash
cd ~/nordic-unicorn-news

# Initialize package.json with proper dependencies
npm init -y

# Install Express backend dependencies
npm install express cors dotenv pg

# Install Svelte frontend dependencies
npm install -D vite @sveltejs/vite-plugin-svelte svelte

# Install dev tools
npm install -D nodemon concurrently
```

**🟢 GREEN: Validate Installation**
```bash
npm list express
npm list svelte
npm list pg
# All should show installed versions
```

**🔵 CHECKPOINT:**
- [ ] Dependencies installed
- [ ] package.json updated
- [ ] Commit: "chore: add Phase 2 dependencies"

---

### Step 2: Create Database Schema (30 min)

**🔴 RED: Create PostgreSQL Container**
```bash
# Update docker-compose.yml to add PostgreSQL
```

**docker-compose.yml addition:**
```yaml
services:
  # ... existing nordic-unicorn-news service ...

  postgres:
    image: postgres:16-alpine
    container_name: nordic-unicorn-news-db
    environment:
      POSTGRES_DB: unicorn_news
      POSTGRES_USER: unicorn
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - "5433:5432"  # Use 5433 to avoid conflicts
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U unicorn -d unicorn_news"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

**🔴 RED: Create Migration File**
```bash
mkdir -p database
```

Create `database/init.sql` with the schema above (companies, articles, users, watchlists tables).

**🟢 GREEN: Validate Database**
```bash
# Start PostgreSQL container
docker compose up -d postgres

# Wait for health check
docker ps --filter "name=postgres"

# Test connection
docker exec -it nordic-unicorn-news-db psql -U unicorn -d unicorn_news -c "\dt"
# Should show: companies, articles, users, watchlists

# Test insert
docker exec -it nordic-unicorn-news-db psql -U unicorn -d unicorn_news -c "
INSERT INTO companies (name, ticker, market, sector, country)
VALUES ('Test Company', 'TEST', 'Nasdaq OMX Stockholm', 'Defence', 'Sweden');
"

# Verify
docker exec -it nordic-unicorn-news-db psql -U unicorn -d unicorn_news -c "SELECT * FROM companies;"
# Should show the test company
```

**🔵 CHECKPOINT:**
- [ ] PostgreSQL container running
- [ ] Schema created and validated
- [ ] Tables exist and accept data
- [ ] Commit: "feat: add PostgreSQL database with schema"

---

### Step 3: Build Express API Skeleton (1 hour)

**🔴 RED: Create API Structure**
```bash
mkdir -p src/server/{routes,models,services,middleware}
```

**Create `src/server/index.js`:**
```javascript
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';
import pg from 'pg';

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3002;

// Database connection
const pool = new pg.Pool({
  host: 'postgres',  // Docker service name
  port: 5432,
  database: 'unicorn_news',
  user: 'unicorn',
  password: process.env.DB_PASSWORD
});

// Middleware
app.use(cors());
app.use(express.json());

// Health check
app.get('/api/health', async (req, res) => {
  try {
    await pool.query('SELECT 1');
    res.json({ status: 'healthy', database: 'connected' });
  } catch (error) {
    res.status(500).json({ status: 'unhealthy', error: error.message });
  }
});

// Routes
app.get('/api/companies', async (req, res) => {
  try {
    const result = await pool.query('SELECT * FROM companies ORDER BY created_at DESC LIMIT 20');
    res.json(result.rows);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.get('/api/articles', async (req, res) => {
  try {
    const result = await pool.query(`
      SELECT a.*, c.name as company_name
      FROM articles a
      LEFT JOIN companies c ON a.company_id = c.id
      ORDER BY a.published_at DESC
      LIMIT 20
    `);
    res.json(result.rows);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Newsletter signup
app.post('/api/newsletter/signup', async (req, res) => {
  const { email, sectors, geography, frequency } = req.body;

  try {
    const result = await pool.query(
      'INSERT INTO users (email, sectors, geography, frequency) VALUES ($1, $2, $3, $4) RETURNING id',
      [email, sectors || [], geography || [], frequency || 'weekly']
    );
    res.json({ success: true, userId: result.rows[0].id });
  } catch (error) {
    if (error.code === '23505') {  // Unique violation
      res.status(400).json({ error: 'Email already subscribed' });
    } else {
      res.status(500).json({ error: error.message });
    }
  }
});

app.listen(PORT, () => {
  console.log(`API server running on port ${PORT}`);
});
```

**Update package.json scripts:**
```json
{
  "type": "module",
  "scripts": {
    "dev:api": "nodemon src/server/index.js",
    "dev:frontend": "vite",
    "dev": "concurrently \"npm run dev:api\" \"npm run dev:frontend\""
  }
}
```

**🟢 GREEN: Validate API**
```bash
# Start API
npm run dev:api

# Test health endpoint
curl http://localhost:3002/api/health
# Expected: {"status":"healthy","database":"connected"}

# Test companies endpoint
curl http://localhost:3002/api/companies
# Expected: [] or test data

# Test newsletter signup
curl -X POST http://localhost:3002/api/newsletter/signup \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","sectors":["Defence"],"frequency":"weekly"}'
# Expected: {"success":true,"userId":1}

# Verify in database
docker exec -it nordic-unicorn-news-db psql -U unicorn -d unicorn_news -c "SELECT * FROM users;"
# Should show test@example.com
```

**🔵 CHECKPOINT:**
- [ ] Express API running
- [ ] Database connection working
- [ ] All endpoints responding
- [ ] Commit: "feat: add Express API with PostgreSQL integration"

---

### Step 4: Integrate ydun-scraper (30 min)

**🔴 RED: Create Scraper Service**

**Create `src/server/services/scraper.js`:**
```javascript
export async function fetchArticle(url) {
  try {
    const response = await fetch('https://scrape.kitt.agency/scrape', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ urls: [url] })
    });

    const data = await response.json();
    return data.results[0];  // Return first result
  } catch (error) {
    console.error('Scraper error:', error);
    throw error;
  }
}
```

**Add endpoint to `src/server/index.js`:**
```javascript
import { fetchArticle } from './services/scraper.js';

app.post('/api/scrape', async (req, res) => {
  const { url } = req.body;

  try {
    const article = await fetchArticle(url);
    res.json(article);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

**🟢 GREEN: Validate Scraper Integration**
```bash
# Test with a real Nordic news URL
curl -X POST http://localhost:3002/api/scrape \
  -H "Content-Type: application/json" \
  -d '{"url":"https://www.breakit.se/artikel/example"}'

# Should return scraped content from ydun-scraper
```

**🔵 CHECKPOINT:**
- [ ] Scraper service integrated
- [ ] Can fetch articles from ydun-scraper
- [ ] Commit: "feat: integrate ydun-scraper for article fetching"

---

### Step 5: Build Svelte Frontend Skeleton (2 hours)

**🔴 RED: Initialize Svelte App**

**Create `vite.config.js`:**
```javascript
import { defineConfig } from 'vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';

export default defineConfig({
  plugins: [svelte()],
  server: {
    port: 5173,
    proxy: {
      '/api': {
        target: 'http://localhost:3002',
        changeOrigin: true
      }
    }
  }
});
```

**Create `src/frontend/App.svelte`:**
```svelte
<script>
  import { onMount } from 'svelte';

  let companies = [];
  let articles = [];

  onMount(async () => {
    // Fetch companies
    const compRes = await fetch('/api/companies');
    companies = await compRes.json();

    // Fetch articles
    const artRes = await fetch('/api/articles');
    articles = await artRes.json();
  });
</script>

<main>
  <header>
    <img src="/assets/logo.svg" alt="Nordic Unicorn News" class="logo">
    <h1>Nordic Unicorn News</h1>
    <p>Following the herds to find the unicorns 🫎</p>
  </header>

  <section class="companies">
    <h2>Recent Nordic IPOs</h2>
    {#if companies.length === 0}
      <p>No companies yet. Add some via the API!</p>
    {:else}
      <div class="company-grid">
        {#each companies as company}
          <div class="company-card">
            <h3>{company.name}</h3>
            <p class="sector">{company.sector}</p>
            <p class="market">{company.market} • {company.country}</p>
          </div>
        {/each}
      </div>
    {/if}
  </section>

  <section class="articles">
    <h2>Latest News</h2>
    {#if articles.length === 0}
      <p>No articles yet. Scrape some via the API!</p>
    {:else}
      <div class="article-list">
        {#each articles as article}
          <article>
            <h3>{article.title}</h3>
            <p class="meta">{article.company_name} • {article.source}</p>
            <p>{article.summary}</p>
          </article>
        {/each}
      </div>
    {/if}
  </section>
</main>

<style>
  /* Nordic aesthetic - dark mode with blues, teals, gold */
  :global(body) {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 50%, #0d9488 100%);
    color: #f1f5f9;
    min-height: 100vh;
  }

  main {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }

  header {
    text-align: center;
    margin-bottom: 4rem;
  }

  .logo {
    width: 150px;
    height: 150px;
  }

  h1 {
    font-size: 3rem;
    margin: 1rem 0;
    background: linear-gradient(135deg, #f59e0b 0%, #0ea5e9 50%, #14b8a6 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .company-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1.5rem;
  }

  .company-card {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    padding: 1.5rem;
    border-radius: 1rem;
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .sector {
    color: #f59e0b;
    font-weight: 600;
  }

  .article-list article {
    background: rgba(255, 255, 255, 0.05);
    padding: 1.5rem;
    margin-bottom: 1rem;
    border-radius: 0.5rem;
  }

  .meta {
    color: #94a3b8;
    font-size: 0.9rem;
  }
</style>
```

**Create `src/frontend/main.js`:**
```javascript
import App from './App.svelte';

const app = new App({
  target: document.getElementById('app')
});

export default app;
```

**Create `index.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nordic Unicorn News</title>
</head>
<body>
  <div id="app"></div>
  <script type="module" src="/src/frontend/main.js"></script>
</body>
</html>
```

**🟢 GREEN: Validate Frontend**
```bash
# Start frontend dev server
npm run dev:frontend

# Open browser to http://localhost:5173
# Should see Nordic Unicorn News UI
# Should show "No companies yet" (empty state)
```

**🔵 CHECKPOINT:**
- [ ] Svelte frontend running
- [ ] Connects to API via proxy
- [ ] Displays companies and articles
- [ ] Commit: "feat: add Svelte frontend with Nordic aesthetic"

---

### Step 6: Add Seed Data (15 min)

**🔴 RED: Create Seed Script**

**Create `database/seed.sql`:**
```sql
-- Sample Nordic companies
INSERT INTO companies (name, ticker, market, sector, country, listing_date, description) VALUES
('Nordic Defence Systems', 'NDS', 'Nasdaq OMX Stockholm', 'Defence', 'Sweden', '2024-06-15', 'AI-powered surveillance systems for Nordic defense'),
('Aurora AI Labs', 'AURA', 'First North', 'AI', 'Norway', '2024-08-20', 'Enterprise AI infrastructure for Nordic companies'),
('Northern Mining Tech', 'NMT', 'Nasdaq OMX Oslo', 'Mining', 'Norway', '2024-05-10', 'AI-powered mineral exploration and automation');

-- Sample articles
INSERT INTO articles (company_id, title, summary, content, source, source_url, published_at, ai_generated) VALUES
(1, 'Nordic Defence Systems raises €50M for AI surveillance',
 'Swedish defence tech company secures major funding round from Nordic VCs',
 'Full article content here...',
 'Breakit',
 'https://www.breakit.se/example',
 '2024-10-15 10:00:00',
 false),
(2, 'Aurora AI Labs announces IPO plans for Q1 2025',
 'Norwegian AI startup targets First North listing',
 'Full article content here...',
 'DI Digital',
 'https://www.di.se/example',
 '2024-10-18 14:30:00',
 false);
```

**🟢 GREEN: Load Seed Data**
```bash
docker exec -i nordic-unicorn-news-db psql -U unicorn -d unicorn_news < database/seed.sql

# Verify
curl http://localhost:3002/api/companies | jq
# Should show 3 companies

curl http://localhost:3002/api/articles | jq
# Should show 2 articles

# Check frontend at http://localhost:5173
# Should now show companies and articles
```

**🔵 CHECKPOINT:**
- [ ] Seed data loaded
- [ ] Companies visible in UI
- [ ] Articles visible in UI
- [ ] Commit: "feat: add seed data for development"

---

### Step 7: Production Docker Setup (1 hour)

**🔴 RED: Create Production Dockerfile**

**Update `Dockerfile` for full-stack:**
```dockerfile
# Build stage for frontend
FROM node:20-alpine AS frontend-builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY src/frontend ./src/frontend
COPY index.html vite.config.js ./
RUN npm run build

# Build stage for backend
FROM node:20-alpine AS backend-builder
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY src/server ./src/server

# Production stage
FROM node:20-alpine
WORKDIR /app

# Copy backend
COPY --from=backend-builder /app/node_modules ./node_modules
COPY --from=backend-builder /app/src/server ./src/server
COPY package.json ./

# Copy built frontend
COPY --from=frontend-builder /app/dist ./public

# Install nginx for serving frontend
RUN apk add --no-cache nginx

# Copy nginx config
COPY docker/nginx.conf /etc/nginx/http.d/default.conf

# Expose ports
EXPOSE 80 3002

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget --quiet --tries=1 --spider http://localhost/health || exit 1

# Start both nginx and Node.js
CMD nginx && node src/server/index.js
```

**Update `docker-compose.yml`:**
```yaml
services:
  nordic-unicorn-news:
    build: .
    container_name: nordic-unicorn-news
    ports:
      - "3001:80"
    environment:
      - NODE_ENV=production
      - DB_PASSWORD=${DB_PASSWORD}
    depends_on:
      - postgres
    restart: unless-stopped

  postgres:
    image: postgres:16-alpine
    container_name: nordic-unicorn-news-db
    environment:
      POSTGRES_DB: unicorn_news
      POSTGRES_USER: unicorn
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/01-schema.sql
      - ./database/seed.sql:/docker-entrypoint-initdb.d/02-seed.sql
    restart: unless-stopped

volumes:
  postgres_data:
```

**Create `.env.example`:**
```bash
DB_PASSWORD=your_secure_password_here
NODE_ENV=production
```

**🟢 GREEN: Validate Production Build**
```bash
# Create .env file
cp .env.example .env
# Edit .env and set secure password

# Build and start
docker compose down
docker compose up -d --build

# Wait for containers to be healthy
docker ps

# Test API
curl http://localhost:3001/api/health
curl http://localhost:3001/api/companies

# Test frontend
curl http://localhost:3001
# Should return HTML

# Test in browser
# http://localhost:3001
# Should show full Nordic Unicorn News site
```

**🔵 CHECKPOINT:**
- [ ] Production build working
- [ ] Both frontend and API accessible
- [ ] Database connected and seeded
- [ ] Commit: "feat: add production Docker setup with multi-stage build"

---

### Step 8: Deploy to Production (30 min)

**🔴 RED: Deploy Updated Stack**
```bash
# Stop old Coming Soon container
docker stop nordic-unicorn-news
docker rm nordic-unicorn-news

# Deploy new full-stack version
docker compose up -d --build

# Verify all containers healthy
docker ps
docker compose ps
```

**🟢 GREEN: Validate Production Deployment**
```bash
# Test locally
curl http://localhost:3001/api/health
curl http://localhost:3001/api/companies
curl http://localhost:3001

# Cloudflare Tunnel is already configured, should work automatically

# Test public URL
curl https://nordicunicornnews.com/api/health
curl https://nordicunicornnews.com/api/companies

# Open browser to https://nordicunicornnews.com
# Should show full Nordic Unicorn News site with companies and articles
```

**🔵 CHECKPOINT:**
- [ ] Full-stack app deployed to production
- [ ] https://nordicunicornnews.com shows new site
- [ ] API endpoints working
- [ ] Database persistent across restarts
- [ ] Commit: "deploy: Phase 2 full-stack app to production"

---

## 📊 Success Criteria Validation

Before marking Phase 2 complete, verify:

- [ ] Users can browse Nordic IPO companies
- [ ] Users can read news articles
- [ ] Newsletter signup stores to database
- [ ] ydun-scraper integration works
- [ ] Site deployed at https://nordicunicornnews.com
- [ ] Database persists across container restarts
- [ ] All endpoints return expected data
- [ ] Frontend displays data from API
- [ ] Health checks passing

---

## 🔄 Rollback Procedure

If Phase 2 deployment fails:

```bash
# Stop new containers
docker compose down

# Restore Coming Soon page
git checkout main^  # Previous commit
docker compose up -d --build

# Verify Coming Soon page is back
curl https://nordicunicornnews.com
```

---

## 📝 Documentation Updates (After Success)

Update these files:
- [ ] STATUS.md - Mark Phase 2 complete
- [ ] NEXT-SESSION-START-HERE.md - Update with Phase 2 details
- [ ] README.md - Update with API documentation
- [ ] AGENTS.md - Add new dependencies and commands

---

## 🎯 Next Phase Preview

**Phase 3: Intelligence Pipeline**
- Connect to real Nordic news sources
- AI-powered article generation (Claude API)
- Auto-translation (Swedish → English)
- Newsletter delivery system
- Watchlist functionality

---

**Created by:** Chromebook Orchestrator
**For execution by:** Beast
**Expected duration:** 6-8 hours
**Follow:** Jimmy's Workflow (RED → GREEN → CHECKPOINT)

**Good luck, Beast! 🫎**
