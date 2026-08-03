<!--
License: CC BY-NC-SA 4.0 (https://creativecommons.org/licenses/by-nc-sa/4.0/)
Copyright (c) 2026 Colin Miley � AI Revenue Systems
Commercial use requires permission: colin@colinmiley.com
-->
# SaaS Identifier

**Version:** 1.0  
**Date:** 2026-08-03  
**Purpose:** Systematically identify, evaluate, and prioritise SaaS opportunities for FDEs (Forward Deployed Engineers), consultants, and solopreneurs. Distinguishes between viable SaaS ideas and services that should remain consultancies.

**Source:** Adapted from `saas-pipeline` (ThomasPraun), FDE research, build-orchestrator, and claude-mode-orchestrator skills.

---

## When to Use This Skill

Use this skill when:
- A client problem appears to be recurring across multiple engagements
- You wonder "could this be a product instead of a service?"
- You're evaluating whether to productize a consulting offering
- You're assessing market opportunity for a SaaS idea
- You need to decide: "consulting retainer vs. SaaS subscription"

**Trigger phrases:** "should I productize this?", "is this a SaaS opportunity?", "can I turn this into a product?", "recurring client problem", "build once, sell many"

---

## The SaaS Identifier Framework

### Core Principle

> "Not every problem should be a SaaS. Not every SaaS should be built."

The framework distinguishes three paths:

| Path | What It Is | When to Choose |
|------|-----------|----------------|
| **Service** | Custom work per client | Problem is unique per client; high touch required |
| **Productized Service** | Fixed-scope, fixed-price engagements | Problem is repeatable but implementation varies |
| **SaaS** | Self-serve software product | Problem is universal; solution is standardised |

---

## Phase 1: Problem Identification (RESEARCH Agent)

### Step 1.1: Document the Recurring Problem

**Question:** Have you encountered this problem 3+ times with different clients?

**If NO:** → Stay as custom service. Not enough data.

**If YES:** → Proceed to Step 1.2.

**Documentation template:**

```markdown
## Recurring Problem Log

### Problem Statement
[One sentence: what breaks, where, for whom]

### Frequency
- Client 1: [date, context]
- Client 2: [date, context]
- Client 3: [date, context]

### Current Workarounds
[What clients do instead of a proper solution]

### Cost of Inaction
[What happens if they do nothing]
```

### Step 1.2: Problem Classification

Classify the problem using the ICE framework:

| Criteria | Score (1-10) | Evidence |
|----------|-------------|----------|
| **Impact** | How much does this problem hurt? | Time lost, money lost, deals missed |
| **Frequency** | How often does it occur? | Daily, weekly, monthly |
| **Ease of Fix** | How hard is it to solve? | Technical complexity, integration requirements |

**Scoring:**
- 24-30: High-priority SaaS candidate
- 15-23: Medium-priority, productized service first
- <15: Low-priority, stay as custom service

---

## Phase 2: Market Validation (ANALYST Agent)

### Step 2.1: Market Size Estimation

**TAM-SAM-SOM Analysis:**

| Metric | Calculation | Example |
|--------|-------------|---------|
| **TAM** | Total addressable market | All B2B sales teams globally |
| **SAM** | Serviceable addressable market | SMB sales teams in UK/Ireland |
| **SOM** | Serviceable obtainable market | Teams you can reach in Year 1 |

**Rule:** SOM must be >100 potential customers for SaaS viability.

### Step 2.2: Competitive Landscape

**Map competitors on two axes:**
- X-axis: Price (low to high)
- Y-axis: Functionality (basic to advanced)

**Identify the gap:** Where are competitors NOT serving?

| Competitor Type | Risk Level | Your Advantage |
|----------------|------------|----------------|
| None | 🔴 High (may mean no market) | First-mover |
| 1-2 | 🟡 Medium | Differentiation required |
| 3-5 | 🟢 Low (validated market) | Niche down or out-execute |
| 10+ | 🔴 High (red ocean) | Don't enter without 10x advantage |

### Step 2.3: Customer Willingness to Pay

**Test BEFORE building:**

1. **Landing page test:** Describe the SaaS, collect email signups
2. **Price anchor test:** Present 3 pricing tiers, see which gets most clicks
3. **Presale test:** Offer "founding member" discount for early signups

**Minimum validation:** 50 email signups or 5 pre-sales before building.

---

## Phase 3: Technical Feasibility (TECHNICAL Agent)

### Step 3.1: Solution Architecture

**Map the solution:**

```
[User] → [Interface] → [Logic] → [Data] → [Integrations]
```

**Questions:**
- Can the core logic be standardised? (Yes = SaaS, No = Service)
- Does it require client-specific configuration? (High = Productized Service)
- Are integrations standardised (APIs) or custom (scrapers)?

### Step 3.2: Build vs. Buy vs. Integrate

| Component | Build | Buy | Integrate |
|-----------|-----|-----|-----------|
| Auth | ❌ Use Auth0/Clerk/Supabase | ✅ | — |
| Database | ❌ Use managed DB | ✅ | — |
| Payments | ❌ Use Stripe | ✅ | — |
| Email | ❌ Use SendGrid/Resend | ✅ | — |
| AI/ML | ⚠️ Use API (OpenAI/Anthropic) | — | ✅ |
| Core logic | ✅ This is your IP | ❌ | ❌ |

**Rule:** Only build what differentiates you. Buy everything else.

### Step 3.3: MVP Scope

**Define the thinnest viable product:**

| Feature | Must-Have (MVP) | Nice-to-Have (v2) |
|---------|----------------|-------------------|
| Core functionality | ✅ | — |
| User auth | ✅ | — |
| Payments | ✅ | — |
| Admin dashboard | ⚠️ | ✅ |
| Reporting | ❌ | ✅ |
| Integrations | Top 1-2 only | More later |
| Mobile app | ❌ | ✅ |

**Time box:** MVP must be buildable in 4-6 weeks by 1 developer.

---

## Phase 4: Business Model Design (STRATEGIST Agent)

### Step 4.1: Pricing Model Selection

| Model | When to Use | Example |
|-------|-------------|---------|
| **Freemium** | Network effects, viral growth | Dropbox, Slack |
| **Usage-based** | Variable consumption | AWS, Stripe |
| **Per-seat** | Team tools | Notion, Figma |
| **Flat subscription** | Predictable value | Basecamp, ConvertKit |
| **Transaction fee** | Facilitating transactions | Shopify, Gumroad |

**For FDE-derived SaaS:** Flat subscription or per-seat typically works best.

### Step 4.2: Unit Economics

**Calculate before building:**

| Metric | Target | Calculation |
|--------|--------|-------------|
| **CAC** (Customer Acquisition Cost) | < 1/3 of LTV | Marketing spend / customers acquired |
| **LTV** (Lifetime Value) | > 3x CAC | ARPU x gross margin x avg lifespan |
| **Payback period** | < 12 months | CAC / monthly gross margin |
| **Churn** | < 5% monthly | Customers lost / total customers |

**If you can't estimate these, don't build yet.**

### Step 4.3: FDE-to-SaaS Transition Path

**For FDEs considering SaaS:**

| Phase | Action | Timeline |
|-------|--------|----------|
| **Phase 0** | FDE engagements (custom) | Months 1-6 |
| **Phase 1** | Productized service (fixed scope) | Months 3-9 |
| **Phase 2** | Hybrid (service + software) | Months 6-12 |
| **Phase 3** | SaaS (self-serve) | Months 9-18 |

**Critical insight:** SaaS ideas should emerge FROM services, not replace them.

---

## Phase 5: Decision Gate (DECISION Agent)

### The SaaS Identifier Scorecard

Score each criterion (1-5):

| # | Criteria | Score | Notes |
|---|----------|-------|-------|
| 1 | Recurring problem (3+ clients) | | |
| 2 | Market size >100 potential customers | | |
| 3 | Competitors exist but gap identified | | |
| 4 | Solution can be standardised | | |
| 5 | MVP buildable in 4-6 weeks | | |
| 6 | CAC < 1/3 LTV (estimated) | | |
| 7 | You have domain expertise | | |
| 8 | You have initial customer base | | |
| 9 | Willing to support users (not just build) | | |
| 10 | Revenue target justifies effort | | |

**Scoring:**
- **40-50:** Strong SaaS candidate. Proceed to MVP.
- **30-39:** Moderate candidate. Productized service first, SaaS later.
- **20-29:** Weak candidate. Stay as custom service or productized service.
- **<20:** Not a SaaS. Don't build.

### Decision Matrix

| Score | Decision | Next Action |
|-------|----------|-------------|
| 40-50 | **BUILD SAAS** | Create landing page, validate demand, build MVP |
| 30-39 | **PRODUCTIZED SERVICE** | Fix scope, fix price, build case studies |
| 20-29 | **CUSTOM SERVICE** | Stay consultative, document for future |
| <20 | **PASS** | Focus on other opportunities |

---

## Phase 6: Validation Actions (EXECUTION Agent)

### If BUILD SAAS:

1. Create landing page with email capture
2. Run ads or content to validate demand (€100-€500 test budget)
3. If 50+ signups: build MVP
4. If <50 signups: pivot or abandon

### If PRODUCTIZED SERVICE:

1. Define fixed scope and fixed price
2. Create sales page
3. Offer to existing clients first
4. Document for future SaaS evolution

### If CUSTOM SERVICE:

1. Continue as-is
2. Document recurring patterns
3. Re-evaluate in 6 months

---

## Anti-Patterns

### Anti-Pattern 1: "Build It and They Will Come"

**Symptom:** No validation. No signups. Just building.
**Fix:** Landing page test FIRST. Build SECOND.

### Anti-Pattern 2: "Everything Should Be SaaS"

**Symptom:** Forcing SaaS model on problems that need human judgment.
**Fix:** Some problems are inherently services. That's fine.

### Anti-Pattern 3: "I Can Build It Myself"

**Symptom:** Underestimating time, overestimating technical ability.
**Fix:** Time-box MVP to 4-6 weeks. If you can't, scope down.

### Anti-Pattern 4: "My Service Clients Will Become SaaS Customers"

**Symptom:** Assuming service clients want self-serve software.
**Reality:** They may prefer your expertise, not your tool.
**Fix:** Test willingness to pay for software separately.

---

## Example: Applying to AI Revenue Systems

| Criteria | Score | Notes |
----------|-------|-------|
| Recurring problem | 5 | Admin waste is universal in B2B sales |
| Market size | 4 | Millions of B2B sales teams |
| Competitors | 3 | Salesforce, Gong, Outreach exist but gap in SMB |
| Standardisable | 3 | Audit process yes, implementation varies |
| MVP buildable | 4 | Quiz + scoring + PDF = 4-6 weeks |
| CAC/LTV | 3 | Unknown without testing |
| Domain expertise | 5 | 15 years in sales |
| Customer base | 3 | None yet, building pipeline |
| Support willingness | 4 | Yes, but solo operator |
| Revenue target | 4 | €5K MRR = €60K ARR, justifies effort |

**Total: 38/50**

**Decision:** PRODUCTIZED SERVICE first, SaaS later. The audit can be productized (€2,150 fixed scope). The SaaS (quiz platform) comes after 50+ audits reveal patterns.

---

## Integration with Other Skills

| Skill | How It Integrates |
-------|-------------------|
| `build-orchestrator` | SaaS Identifier runs BEFORE build authorization |
| `claude-mode-orchestrator` | Deep Research mode for market validation |
| `content-authenticity-enforcer` | Verify all market claims in scorecard |
| `pricing` | Pricing model selection and unit economics |
| `cro` | Landing page validation and conversion |
| `cold-email` | Customer discovery outreach |

---

## Session Output

After running SaaS Identifier, create:

```
saas-opportunity/[problem-name]/
├── 01-problem-log.md
├── 02-market-analysis.md
├── 03-technical-feasibility.md
├── 04-business-model.md
├── 05-scorecard.md
└── 06-decision.md
```

---

*Skill created: 2026-08-03*  
*Sources: saas-pipeline (ThomasPraun), FDE research, build-orchestrator, claude-mode-orchestrator, content-authenticity-enforcer*
