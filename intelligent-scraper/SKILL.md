# Intelligent Scraper

**Structured web extraction using natural language prompts — adapted from ScrapeGraphAI patterns.**

Extract structured data from web pages without writing selectors. Uses a three-tier approach: defuddle for simple text, model-assisted for structured single-page, and parallel subagents for batch multi-page extraction.

---

## What This Is

An adaptation of ScrapeGraphAI's graph-based scraping pipeline, reimplemented using OpenClaw's existing tools (`kimi_fetch`, `kimi_search`, `defuddle`, `sessions_spawn`). No Python or Playwright dependencies.

**Source inspiration:** [ScrapeGraphAI/Scrapegraph-ai](https://github.com/ScrapeGraphAI/Scrapegraph-ai) (30.3k stars)

---

## Three-Tier Approach

### Tier 1: Defuddle (Fast, Free, Markdown)
Use for: Clean text from single pages — articles, docs, blog posts  
Cost: $0 | Speed: <2s | Output: Markdown

```bash
defuddle https://example.com/article > article.md
```

**When to use:**
- Reading article content for curation
- Extracting documentation
- Getting clean text from any page

**When NOT to use:**
- Need structured data (pricing, features, contacts)
- Multiple pages to compare
- Data needs validation

---

### Tier 2: Model-Assisted Structured Extraction
Use for: Structured JSON from single pages — pricing, features, contacts  
Cost: ~500-2K tokens | Speed: <5s | Output: JSON

```
STEP 1: kimi_fetch URL → raw content
STEP 2: Model prompt → "Extract {fields} from this content. Return JSON."
STEP 3: Validate JSON schema
STEP 4: Save to output/
```

**Example prompt:**
```
Extract pricing information from this page. Return JSON:
{
  "company_name": "string",
  "pricing_tiers": [
    {
      "name": "string",
      "price": "number",
      "currency": "string",
      "billing": "monthly|annual",
      "features": ["string"]
    }
  ],
  "notes": "string"
}
```

**When to use:**
- Competitor pricing extraction
- Feature comparison
- Contact info parsing
- Product catalog extraction

---

### Tier 3: Parallel Batch Extraction
Use for: Multi-page batch processing — 5+ URLs, search-based research  
Cost: Variable | Speed: Minutes | Output: JSON array

```
STEP 1: Define URL list (manual, search results, or sitemap)
STEP 2: Spawn subagent per URL (parallel via sessions_spawn)
STEP 3: Each subagent runs Tier 2 extraction
STEP 4: Parent aggregates results
STEP 5: Validate and save to output/
```

**Example:**
```
URLs: [
  "https://competitor1.com/pricing",
  "https://competitor2.com/pricing",
  "https://competitor3.com/pricing"
]
Prompt: "Extract pricing tiers for all companies. Use consistent schema."
Output: competitors-pricing.json
```

**When to use:**
- Batch competitor analysis
- Lead list building
- Multi-source research
- Catalog extraction

---

## Extraction Patterns

### Pattern: Competitor Pricing

```
Input: competitor URL
Output:
{
  "company": "Competitor Name",
  "url": "https://...",
  "pricing": [
    {
      "tier": "Starter",
      "price": 49,
      "currency": "EUR",
      "billing": "monthly",
      "features": ["Feature 1", "Feature 2"]
    }
  ],
  "positioning": "One-line positioning statement",
  "guarantee": "Money-back terms",
  "confidence": "high"
}
```

### Pattern: Lead Extraction

```
Input: Company LinkedIn or website URL
Output:
{
  "company_name": "...",
  "website": "...",
  "industry": "...",
  "size": "11-50",
  "location": "Dublin, Ireland",
  "key_contacts": [
    {
      "name": "...",
      "title": "...",
      "linkedin": "..."
    }
  ],
  "recent_news": "...",
  "hiring": true,
  "confidence": "medium"
}
```

### Pattern: Content Research

```
Input: Article or research paper URL
Output:
{
  "title": "...",
  "author": "...",
  "publication_date": "...",
  "key_claims": ["...", "..."],
  "statistics": [
    {"claim": "...", "value": "...", "source": "..."}
  ],
  "relevance_score": 8,
  "confidence": "high"
}
```

### Pattern: Market Intelligence

```
Input: Industry news URL or search query
Output:
{
  "topic": "...",
  "sources": ["url1", "url2"],
  "summary": "...",
  "key_players": ["..."],
  "trends": ["..."],
  "implications": "..."
}
```

---

## Validation Rules

Every extraction must pass:

- [ ] **JSON valid:** Parseable, no syntax errors
- [ ] **Schema complete:** All required fields present
- [ ] **Types correct:** Numbers are numbers, arrays are arrays
- [ ] **No placeholders:** Reject "N/A", "Coming soon", "TBD"
- [ ] **Source recorded:** Original URL in `source_url`
- [ ] **Timestamped:** `extracted_at` in ISO 8601
- [ ] **Confidence rated:** `high` | `medium` | `low`
- [ ] **Notes included:** Any caveats, missing data, or assumptions

---

## ICM Integration

### Stage Placement

```
stages/
  02-competitor-research/
    CONTEXT.md              # "Research 5 competitor pricing pages"
    references/
      scraper-patterns.md   # This skill
    output/
      competitor-data.json  # Extracted data
      learnings.md          # What worked, what didn't
```

### Agent Workflow

**Single-page extraction:**
```
1. Read stage CONTEXT.md
2. Load references/scraper-patterns.md
3. kimi_fetch URL
4. Model extract with prompt
5. Validate JSON
6. Save to output/
7. Append learnings
```

**Batch extraction (Ralph pattern):**
```
1. Read stage CONTEXT.md
2. Load references/scraper-patterns.md
3. Define URL list in stage-contract.json
4. For each URL: spawn subagent → extract → validate
5. Aggregate results
6. Save to output/
7. Append learnings
```

---

## Anti-Patterns

| Anti-Pattern | Fix |
|-------------|-----|
| Use Tier 3 for single simple page | Use Tier 1 (defuddle) or Tier 2 |
| No validation | Always run validation checklist |
| Hardcode CSS selectors | Use NL prompts — sites change |
| Ignore robots.txt | Respect crawl policies |
| No rate limiting | Add 1-2s delay between requests |
| Scrape without purpose | Define extraction goal first |
| Store without source | Always record source_url |

---

## Command Reference

```bash
# Tier 1: Defuddle (free, fast)
defuddle https://example.com > output.md

# Tier 2: Model-assisted (single page)
# Use kimi_fetch + model prompt (see patterns above)

# Tier 3: Batch (parallel subagents)
# Use sessions_spawn per URL (see Ralph execution skill)

# Search → Extract pipeline
# 1. kimi_search "query"
# 2. Extract URLs from results
# 3. Spawn subagents for each URL
```

---

## Cost Guidelines

| Tier | Use Case | Cost | Speed |
|------|----------|------|-------|
| 1 | Single page → markdown | $0 | <2s |
| 2 | Single page → structured JSON | ~500-2K tokens | <5s |
| 3 | Batch 10 pages → JSON array | ~5K-20K tokens | <2 min |

**Rule of thumb:** Start with Tier 1. Escalate to Tier 2 only when structure matters. Use Tier 3 only for batch jobs.

---

*Integrates with: `icm-core`, `icm-ralph-execution`, `competitor-monitoring`, `research-writing`, `content-pipeline`, `prospect-intelligence`*
