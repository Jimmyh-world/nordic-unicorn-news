# Integration Architecture - READ THIS FIRST

**⚠️ CORE SYSTEM DOCUMENT ⚠️**

Nordic Unicorn News integrates with the dev-rag system. Before implementing, read:

📘 **[INTEGRATION-ARCHITECTURE-BLUEPRINT.md](./INTEGRATION-ARCHITECTURE-BLUEPRINT.md)**

## Your Role: HYBRID Pattern (Provider + Consumer + Publisher)

**Nordic Unicorn News uses MULTIPLE patterns:**

### Pattern 1: Content Provider
Your generated articles become RAG content:
- AI-generated Nordic IPO news
- Structured with YAML frontmatter
- Tagged: trading, nordic-ipo, defence, ai, mining
- Ingested into trading-rag collection

### Pattern 4: Query Consumer  
You query RAG for context:
- "What do we know about this company?"
- "Recent IPOs in Defence sector?"
- "Similar companies to [ticker]?"
- Enriches AI-generated articles with historical context

### Pattern 2: Event Publisher (Optional)
You can publish IPO events:
- New listing detected → publish to trading-events
- Company profile updated → trigger re-indexing
- User watchlist spikes → trading signal

## Your Tech Stack Integration

**Already planned:**
- ✅ Qdrant (vector search) - Use shared dev-rag instance
- ✅ Kafka (event streaming) - Use shared event bus
- ✅ Claude API (AI) - Use for generation + RAG queries
- ✅ Guardian Pi - Monitor RSS feeds for you

**Integration points:**
- RSS Monitor (Guardian) → Kafka → Your Writer Agent → RAG
- Your articles → RAG ingestion → Queryable knowledge base
- User watchlists → RAG query → Related companies/insights

## How RAG Helps You

### Use Case 1: AI Article Generation

```python
from dev_rag_client import RAGClient

client = RAGClient()

# New IPO detected: Svenska Defence Tech AB
# Query RAG for context
result = client.query(
    "Recent Nordic defence sector IPOs and their performance",
    domain="trading",
    filters={
        "category": "ipo",
        "sector": "defence",
        "geography": "nordic"
    }
)

# Use RAG context in your AI article generation
article_prompt = f"""
Write a news article about Svenska Defence Tech AB IPO.

Context from similar IPOs:
{result['answer']}

Generate article focusing on:
- Company background
- Defence sector trends
- Comparison to recent Nordic defence IPOs
"""

# Your Writer agent generates informed article
# Sources cited from RAG
```

### Use Case 2: Company Profile Enrichment

```python
# User clicks on "Saab AB" company profile
# Query RAG for all mentions
result = client.query(
    "Saab AB recent news, IPO history, and sector analysis",
    domains=["trading", "security"],  # Defence is in both!
    filters={"ticker": "SAAB", "geography": "sweden"}
)

# Display enriched profile:
# - Recent mentions from RAG
# - Related companies
# - Sector context
# - Historical performance
```

### Use Case 3: Newsletter Personalization

```python
# User subscribed to: Defence sector, Weekly digest
# Query RAG for this week's Defence sector activity
result = client.query(
    "Nordic defence sector activity this week",
    domain="trading",
    filters={
        "sector": "defence",
        "timestamp_after": "2025-10-14"  # Last 7 days
    }
)

# Generate personalized newsletter section
# With AI-summarized highlights from RAG
```

## Integration Architecture for You

```
Guardian Pi → RSS Monitor → Kafka 'dev-events'
                                ↓
                Your Writer Agent (consumes event)
                                ↓
                Generate Article (queries RAG for context)
                                ↓
                Publish to Nordic Unicorn News
                                ↓
                Ingest Article → RAG 'trading-rag' collection
                                ↓
                Now queryable for future articles!
```

## Key Sections for You in Blueprint

- **Pattern 1:** How to structure your articles for RAG
- **Pattern 2:** How to publish IPO events (optional)
- **Pattern 4:** How to query RAG for context
- **News Scraper:** How to scrape Nordic sources
- **RSS Scraper:** How Guardian monitors feeds for you

## Your Metadata Schema

```yaml
---
domain: trading
category: nordic-ipo
tags: [defence, sweden, first-north, listing]
company: Svenska Defence Tech AB
ticker: SDEF
sector: defence
geography: nordic
listing_date: 2025-10-21
market: first-north
version: 1.0.0
date: 2025-10-21
priority: high
---

# Your article content here
```

## Related Projects

- **dev-rag:** Your RAG backend (query for context, ingest articles)
- **dev-guardian:** Monitors RSS feeds, triggers your Writer agent
- **dev-scraping:** Can scrape Nordic sources for you
- **dev-blockchain:** Different domain (not relevant to you)

---

**Nordic Unicorn News is a PERFECT real-world use case for the RAG system!**

You're both a content producer (articles) AND a content consumer (context queries).
