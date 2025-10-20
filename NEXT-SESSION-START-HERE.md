# Nordic Unicorn News - Next Session Start Here

<!--
TEMPLATE_VERSION: 1.0.0
TEMPLATE_SOURCE: /home/jimmyb/templates/NEXT-SESSION-START-HERE.md.template
LAST_SYNC: 2025-10-20
PURPOSE: Provide quick context and continuity between development sessions
-->

**Last Updated:** 2025-10-20 (Session 2 - Strategy Session)
**Last Session:** Pivoted to RAG-first strategy - Nordic news as tracer bullet for dev-rag system
**Current Phase:** Planning Phase 2 - RAG Infrastructure Deployment
**Session Summary:** Made critical architectural decisions, validated approach with dev-rag research

---

## ⚡ Quick Context Load (Read This First!)

### What This Project Is

**Nordic Unicorn News** is a real-time intelligence platform covering Nordic tech IPOs, listings, and innovation in Defence, AI, and Mining sectors.

**NEW STRATEGIC DECISION:** Using Nordic news as **TRACER BULLET** to validate entire dev-rag system!

**Current Status:** 20% complete
- ✅ Project vision documented
- ✅ Branding complete (moose-unicorn logo 🫎)
- ✅ Coming Soon page LIVE at https://nordicunicornnews.com
- ✅ Cloudflare Tunnel operational
- ✅ **RAG strategy designed** - using dev-rag research
- ✅ **AI newsroom architecture planned** - autonomous Editor + Writer agents
- 🔄 **NEXT: Phase 2 execution specs** for Beast

---

## 🎯 MAJOR SESSION 2 DECISIONS (2025-10-20)

### 1. RAG-First Approach
**Decision:** Deploy full RAG infrastructure BEFORE simple database
**Why:** Nordic news is low-stakes validation of entire dev-rag system
**Philosophy:** Build, validate, iterate or throw away - learn fast with low risk

### 2. Autonomous AI Newsroom
**Decision:** Fully autonomous article generation from day 1
- Editor Agent (Sonnet 4.5 orchestrator)
- Writer Agent (Haiku 4.5 generalist)
- NO human review required
**Why:** Test if agents can generate quality content autonomously

### 3. Start Simple
**Decisions:**
- ✅ 1 RSS source (Breakit.se) - not all Nordic sources
- ✅ 1 Writer agent (generalist) - not 3 specialists
- ✅ 1 Kafka topic - not multiple
- ✅ Guardian Pi triggers via RSS monitoring

### 4. Update Strategy
**Decision:** Hybrid approach
- Important news → New article (Writer agent generates)
- Minor updates → Update company metadata only
- Duplicates → Ignore

### 5. Tech Stack Validated
**From dev-rag research:**
- Qdrant (vector database)
- Kafka (event bus)
- Ollama + mxbai-embed-large (embeddings)
- Claude Haiku 4.5 + Sonnet 4.5 (agents)
- ydun-scraper (already deployed)

---

## 🟢 Current Status Summary

### What's Been Completed ✅

**Infrastructure (Session 1):**
- ✅ Coming Soon page deployed
- ✅ Docker + Cloudflare Tunnel working
- ✅ Site LIVE at https://nordicunicornnews.com
- ✅ Beast ready (96GB RAM, 78GB disk free)

**Strategy & Planning (Session 2):**
- ✅ Reviewed dev-rag research (11K lines of docs!)
- ✅ Designed RAG-first architecture
- ✅ Planned AI newsroom (Editor + Writer agents)
- ✅ Created tracer bullet strategy
- ✅ Documented in NORDIC-NEWS-RAG-STRATEGY.md

**Key Documents Created:**
- ✅ docs/PHASE-2-EXECUTION-SPEC.md (original - needs revision)
- ✅ docs/CLOUDFLARE-TUNNEL-SETUP.md
- ✅ docs/DEPLOYMENT.md (from Beast)
- ✅ docs/NORDIC-NEWS-RAG-STRATEGY.md (NEW!)

**Metrics:**
- Site: LIVE and healthy
- Documentation: ~2,500 lines
- Strategy: Complete and validated
- Progress: 20% (was 15%, added strategy +5%)

---

## 🎯 Current Task: Prepare Phase 2 Execution Specs

### What Needs To Happen Before Beast Starts

**Orchestrator Tasks (Chromebook - YOU):**
- [ ] Review/revise Phase 2 execution spec for RAG infrastructure
- [ ] Create Guardian Pi RSS monitoring spec
- [ ] Push specs to GitHub
- [ ] Signal Beast to pull and execute

**Beast Tasks (After specs ready):**
- [ ] Deploy Qdrant container
- [ ] Deploy Kafka container
- [ ] Deploy Ollama + mxbai model
- [ ] Integrate ydun-scraper with pipeline
- [ ] Validate: Can query RAG system

**Guardian Pi Tasks:**
- [ ] Set up RSS monitoring script (Breakit.se)
- [ ] Publish events to Kafka on Beast
- [ ] Validate: Events appear in Kafka

---

## 📁 Key Project Files (Quick Access)

### Core Documentation
1. **AGENTS.md** - Development guidelines and AI context
2. **README.md** - Project overview
3. **STATUS.md** - Project progress tracking (NEEDS UPDATE!)
4. **NEXT-SESSION-START-HERE.md** - This file

### Strategy & Architecture
5. **docs/NORDIC-NEWS-RAG-STRATEGY.md** ⭐ - Complete tracer bullet strategy
6. **docs/PHASE-2-EXECUTION-SPEC.md** - Needs revision for RAG-first
7. **docs/CLOUDFLARE-TUNNEL-SETUP.md** - Tunnel configuration

### Deployment
8. **docker-compose.yml** - Container orchestration (needs RAG additions)
9. **Dockerfile** - Current: nginx static site
10. **cloudflare/config.yml** - Tunnel routing (from Beast)

### Related Research
11. **~/dev-rag/** - Complete RAG system research (11K lines!)
12. **~/dev-rag/specs/RAG-BUILD-SEQUENCE-TDD.md** - TDD implementation guide

---

## 🏗️ Architecture Overview

### Current (Phase 1)
```
User → https://nordicunicornnews.com
    → Cloudflare Tunnel
    → Beast: nginx static "Coming Soon" page
```

### Target (Phase 2 + 3 + 4)
```
┌────────────────────────────────────────────┐
│ Guardian Pi (192.168.68.10)                │
│ - Monitor Breakit.se RSS                   │
│ - Publish to Kafka when IPO detected       │
└──────────────┬─────────────────────────────┘
               ↓ Kafka event
┌──────────────────────────────────────────────────┐
│ Beast (192.168.68.100) - Docker Stack            │
│                                                   │
│ ┌─────────────────────────────────────────────┐ │
│ │ RAG Infrastructure (Phase 2)                │ │
│ │ ├─ Kafka (event bus)                        │ │
│ │ ├─ ydun-scraper (extract content)           │ │
│ │ ├─ Ollama + mxbai (generate embeddings)     │ │
│ │ └─ Qdrant (vector storage)                  │ │
│ └─────────────────────────────────────────────┘ │
│                                                   │
│ ┌─────────────────────────────────────────────┐ │
│ │ AI Newsroom (Phase 3)                       │ │
│ │ ├─ Editor Agent (Sonnet 4.5 orchestrator)   │ │
│ │ └─ Writer Agent (Haiku 4.5 generalist)      │ │
│ └─────────────────────────────────────────────┘ │
│                                                   │
│ ┌─────────────────────────────────────────────┐ │
│ │ Storage (Phase 3)                           │ │
│ │ ├─ PostgreSQL (articles, companies, users)  │ │
│ │ └─ Qdrant (company knowledge, embeddings)   │ │
│ └─────────────────────────────────────────────┘ │
│                                                   │
│ ┌─────────────────────────────────────────────┐ │
│ │ Frontend (Phase 4)                          │ │
│ │ ├─ Svelte (display articles)                │ │
│ │ ├─ Express API (serve data)                 │ │
│ │ └─ nginx (reverse proxy)                    │ │
│ └─────────────────────────────────────────────┘ │
└──────────────┬────────────────────────────────────┘
               ↓
         Cloudflare Tunnel
               ↓
    https://nordicunicornnews.com
```

---

## 🎯 Immediate Next Steps (Choose One)

### Option 1: Finalize Phase 2 Execution Spec ⭐ RECOMMENDED

**Goal:** Create detailed RAG infrastructure deployment spec for Beast

**Tasks:**
- [ ] Review existing PHASE-2-EXECUTION-SPEC.md
- [ ] Revise for RAG-first approach (not simple DB)
- [ ] Add Qdrant deployment steps
- [ ] Add Kafka deployment steps
- [ ] Add Ollama + mxbai deployment steps
- [ ] Add validation commands for each step
- [ ] Push to GitHub

**Why First:** Beast needs this to execute Phase 2

**Estimated Time:** 1-2 hours

---

### Option 2: Create Guardian Pi Monitoring Spec

**Goal:** Spec for Guardian Pi RSS monitoring setup

**Tasks:**
- [ ] Design Python RSS monitoring script
- [ ] Kafka producer configuration
- [ ] Keyword detection logic (IPO, Defence, AI, Mining)
- [ ] Cron job or systemd service setup
- [ ] Validation commands

**Why Next:** Guardian needs to know what to build

**Estimated Time:** 30-60 min

---

### Option 3: Update Project Documentation

**Goal:** Sync STATUS.md and other docs with new strategy

**Tasks:**
- [ ] Update STATUS.md with Phase 2 RAG strategy
- [ ] Update README.md with RAG architecture
- [ ] Review AGENTS.md for accuracy
- [ ] Commit all updates

**Why Next:** Keep documentation accurate

**Estimated Time:** 30 min

---

## 🔑 Quick Reference

### Access Project
```bash
cd ~/unicorn-news

# Check git status
git status
git log --oneline -5

# Read strategy
cat docs/NORDIC-NEWS-RAG-STRATEGY.md

# Check live site
curl https://nordicunicornnews.com
```

### Related Repositories
```bash
# RAG research
cd ~/dev-rag
cat STATUS.md

# Guardian Pi
cd ~/dev-guardian

# Deployment infrastructure
cd ~/dev-network
```

### Live Site Status
```bash
# Test site
curl https://nordicunicornnews.com

# Check container
docker ps --filter "name=nordic-unicorn-news"

# Check tunnel
cloudflared tunnel info nordic-unicorn-news
```

---

## 💡 Key Insights (Session 2 Recap)

### Why RAG-First Makes Sense
**Original plan:** PostgreSQL → Express → Svelte → add RAG later
**New plan:** RAG infrastructure → AI agents → PostgreSQL → Frontend

**Reasoning:**
- Nordic news is LOW STAKES (perfect for testing)
- We ALREADY have dev-rag research (11K lines!)
- Learn what works/breaks with REAL implementation
- If it's shit, throw it away and start over
- Then apply to HIGH STAKES domains (security, trading)

### Why Fully Autonomous Agents
**Question:** Should humans review articles before publishing?
**Answer:** NO - full autonomous from day 1

**Reasoning:**
- Test if agents CAN generate quality autonomously
- Learn failure modes early
- Low stakes = safe to fail
- Easier to add human review later than remove it

### Why Start Simple
**Could build:** 3 specialist writers (Defence/AI/Mining)
**Actually building:** 1 generalist writer

**Reasoning:**
- YAGNI principle (You Ain't Gonna Need It)
- Prove concept with 1 agent first
- Add specialists only if generalist fails
- Simpler = easier to debug

### Resource Expectations (Phase 2)
**Estimated:**
- Qdrant: ~8GB RAM
- Kafka: ~4GB RAM
- Ollama + mxbai: ~12GB RAM
- Total: ~24GB RAM

**Beast Capacity:**
- Total: 96GB RAM
- Used: ~8GB (monitoring, tunnel, etc.)
- Available: ~88GB RAM
- **Result:** ✅ Plenty of capacity

### Cost Expectations (Phase 3)
**50 articles/month:**
- Editor queries: $0.006/article
- Writer generation: $0.007/article
- Total: $0.013/article × 50 = **$0.65/month**

**100 articles/month:** **$1.30/month**

**Conclusion:** Basically FREE to run! 🎉

---

## ⚠️ Important Reminders

### 1. This Is A Tracer Bullet
**Not production yet** - This is LEARNING:
- Validate tech stack works
- Measure actual resource usage
- Test if agents generate quality
- Learn what breaks
- Iterate or throw away

### 2. Start Simple Philosophy
- 1 RSS source (Breakit.se)
- 1 Writer agent (generalist)
- 1 Kafka topic
- Add complexity ONLY if needed

### 3. Jimmy's Workflow (RED → GREEN → CHECKPOINT)
**Every step:**
- 🔴 RED: Deploy/implement
- 🟢 GREEN: Validate with explicit commands
- 🔵 CHECKPOINT: Commit and document rollback

### 4. Low Stakes = Freedom to Fail
**If RAG returns garbage:** Iterate or throw away
**If agents hallucinate:** Adjust or give up
**If resources blow up:** Rethink architecture

**No pressure** - Just learning!

### 5. Then Apply to High-Stakes Domains
**Once validated:**
- Security domain (CVE tracking)
- Trading domain (market analysis)
- Development domain (Cardano docs)

**With confidence** - We know what works!

---

## 🔧 Common Operations

### Session Start
```bash
cd ~/unicorn-news

# Pull latest from GitHub
git pull origin main

# Check what changed
git log --oneline -10

# Read this file
cat NEXT-SESSION-START-HERE.md

# Check live site
curl https://nordicunicornnews.com
```

### During Development
```bash
# Check strategy
cat docs/NORDIC-NEWS-RAG-STRATEGY.md

# Check dev-rag research
cd ~/dev-rag && cat README.md

# Make changes
vim docs/PHASE-2-EXECUTION-SPEC.md

# Commit often
git add -A
git commit -m "description"
git push origin main
```

### Coordination
```bash
# Push specs to GitHub
git push origin main

# Beast pulls and executes
# (on Beast): git pull origin main

# Guardian pulls and executes
# (on Guardian): git pull origin main
```

---

## 📊 Progress Tracking

### Phase 1: Coming Soon Page ✅ COMPLETE
- **Status:** 100% complete
- **Output:** Live at https://nordicunicornnews.com
- **Duration:** 1 hour (2025-10-20)

### Phase 2: RAG Infrastructure ⚪ READY TO START
- **Status:** 0% (specs being finalized)
- **Estimated:** 3-5 days
- **Next:** Create execution spec for Beast

### Phase 3: AI Newsroom ⚪ NOT STARTED
- **Status:** 0% (depends on Phase 2)
- **Estimated:** 5-7 days

### Phase 4: Frontend ⚪ NOT STARTED
- **Status:** 0% (depends on Phase 3)
- **Estimated:** 2-3 days

**Total Progress:** 20% (strategy + Phase 1 complete)

---

## 🔄 Session Continuity Pattern

### At Session Start
1. ✅ Pull latest from GitHub
2. ✅ Read this file (NEXT-SESSION-START-HERE.md)
3. ✅ Check live site status
4. ✅ Review last commit

### During Session
1. ✅ Follow Jimmy's Workflow (RED → GREEN → CHECKPOINT)
2. ✅ Update this file with progress
3. ✅ Commit frequently
4. ✅ Document decisions with dates

### At Session End
1. ✅ Update this file with "What's Next"
2. ✅ Update STATUS.md with progress
3. ✅ Commit all changes
4. ✅ Push to GitHub
5. ✅ Verify deployment still healthy

---

## 📚 Documentation Quick Reference

| Document | Purpose |
|----------|---------|
| NEXT-SESSION-START-HERE.md | Session continuity (this file) |
| STATUS.md | Project progress tracking |
| AGENTS.md | AI assistant development guidelines |
| README.md | Project overview |
| docs/NORDIC-NEWS-RAG-STRATEGY.md | Complete tracer bullet strategy |
| docs/PHASE-2-EXECUTION-SPEC.md | Beast execution plan (needs revision) |
| docs/CLOUDFLARE-TUNNEL-SETUP.md | Tunnel configuration |
| docs/DEPLOYMENT.md | Production deployment guide |

---

## 📋 Session History

### Session 1 - 2025-10-20 (Initial Setup & Domain)
**Duration:** ~2 hours
**Focus:** Project initialization, Coming Soon page deployment, domain purchase

**Accomplishments:**
- Created project vision and templates
- Purchased nordicunicornnews.com
- Deployed Coming Soon page to Beast
- Configured Cloudflare Tunnel
- Site went LIVE!

### Session 2 - 2025-10-20 (RAG Strategy Pivot)
**Duration:** ~2 hours
**Focus:** Strategic planning, dev-rag research review, architecture decisions

**Accomplishments:**
- Reviewed dev-rag research (11K lines of docs)
- Decided on RAG-first approach (not simple DB)
- Designed AI newsroom (Editor + Writer agents)
- Created tracer bullet strategy
- Documented in NORDIC-NEWS-RAG-STRATEGY.md
- Made critical architectural decisions (autonomous, simple, hybrid updates)

**Key Decisions:**
- Use Nordic news as tracer bullet for dev-rag system
- Full autonomous article generation (no human review)
- Start simple (1 RSS source, 1 writer agent)
- Hybrid update strategy (new article vs metadata update)
- Guardian Pi triggers via RSS monitoring

**Next Session:**
- Finalize Phase 2 execution spec for RAG infrastructure
- Create Guardian Pi monitoring spec
- Push specs to GitHub for Beast/Guardian execution

---

## 🎬 Ready to Continue!

**Most Common Next Action:**
```bash
cd ~/unicorn-news
git pull origin main
cat docs/NORDIC-NEWS-RAG-STRATEGY.md
git status
```

**Current Focus:** Finalize execution specs for Phase 2 (RAG infrastructure)

**Philosophy:** Build → Validate → Learn → Iterate or Throw Away

**Good luck!** 🫎

---

**This file is updated at the end of each session for continuity.**

**Last Updated:** 2025-10-20 (End of Session 2)
**Template Version:** 1.0.0
