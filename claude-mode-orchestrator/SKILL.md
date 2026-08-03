<!--
License: CC BY-NC-SA 4.0 (https://creativecommons.org/licenses/by-nc-sa/4.0/)
Copyright (c) 2026 Colin Miley � AI Revenue Systems
Commercial use requires permission: colin@colinmiley.com
-->
# Claude Mode Orchestrator

**Version:** 1.0  
**Date:** 2026-08-02  
**Purpose:** Guide the selection and application of Claude's capability modes (Standard, Fable, Mythos, Deep Research) for content creation, design, and research tasks.

**Source:** Research file `Deep Dive Claude Fable 5 and Claude Mythos 5 and API Access.md`

---

## The Four Claude Modes

Claude can operate in four distinct modes depending on how you prompt it. These are not separate models — they're behavioral modes within Claude Sonnet 5.

### Mode 1: Standard (Default)

**How to invoke:** Default behavior. No special prompt needed.

**What it produces:** Balanced, accurate, helpful output. Good for most tasks.

**Best for:**
- Headlines and taglines
- Email responses
- Code generation
- Data extraction
- Simple explanations

**Cost:** €0.05-0.15 per task

**Limitation:** Can be generic. Lacks creative spark for emotional content.

---

### Mode 2: Fable (Storytelling/Creative)

**How to invoke:**
```
System prompt or instruction:
"Write this as a compelling narrative. Use scene-setting, 
character emotion, and a story arc. Make the reader feel 
the transformation."
```

**What it produces:** Emotionally resonant, narrative-driven content.

**Best for:**
- **Case studies as stories** (not bullet points)
- **Founder origin stories**
- **Client testimonials** (written as transformation narratives)
- **About pages** with emotional depth
- **Brand storytelling**
- **Landing page copy** that converts through emotion

**Example output difference:**

| Standard | Fable |
|----------|-------|
| "We helped a SaaS team reduce admin time by 5×." | "Sarah's best rep was quitting. Not because of the money — because he was spending 10 hours a week in Salesforce instead of talking to prospects. Six weeks later, he sent his first AI-drafted follow-up in under 2 minutes. He didn't quit. He asked for more accounts." |

**Cost:** €0.15-0.25 per story

**When to use:** When you need the reader to *feel* something, not just *understand* something.

---

### Mode 3: Mythos (Systems/World-Building)

**How to invoke:**
```
System prompt or instruction:
"Explain this as an interconnected system. Show how the 
parts relate, the inputs and outputs, the feedback loops. 
Make it feel like a coherent framework that could be taught 
and replicated."
```

**What it produces:** Complex, structured, system-level thinking.

**Best for:**
- **Methodology documentation**
- **Framework explanations** (like the Graph Engineering Framework)
- **Process maps** with interconnections
- **Intellectual property** (your unique system)
- **Complex tool comparisons**
- **Training materials**

**Example output difference:**

| Standard | Mythos |
|----------|--------|
| "The audit has 5 steps: intake, research, analysis, recommendations, delivery." | "The AI Revenue Audit operates as a five-node graph. Intake is the router — it classifies and directs. Research is the fan-out — five parallel data streams. Analysis is the merge — synthesis from divergent inputs. Recommendations are the loop — iterate until quality threshold. Delivery is the terminal — but it's also a new intake for implementation." |

**Cost:** €0.20-0.30 per system doc

**When to use:** When you need to establish intellectual property or explain complex systems.

---

### Mode 4: Deep Research (Evidence-Based)

**How to invoke:**
```
System prompt or instruction:
"Research this topic thoroughly. Use web search. Cite your 
sources. Provide data, statistics, and evidence. Flag any 
claims you can't verify."
```

**Requires:** Claude with Extended Thinking + Web Search enabled

**What it produces:** Researched, cited, evidence-based content.

**Best for:**
- **Industry benchmark reports**
- **Competitive intelligence**
- **ROI calculations** with real data
- **Thought leadership** with citations
- **White papers**
- **Data-driven blog posts**

**Example output difference:**

| Standard | Deep Research |
|----------|---------------|
| "Sales reps spend a lot of time on admin." | "The average B2B sales rep loses 10.5 hours per week to administrative tasks (Salesforce State of Sales Report, 2024). For a team of 12 reps at €75/hour fully loaded cost, that's €9,450 per week — nearly €500,000 annually — spent on activities that don't generate revenue." |

**Cost:** €0.30-0.50 per research task (higher due to Extended Thinking + Web Search)

**When to use:** When credibility requires evidence. When you need to back up claims with data.

---

## Mode Selection Matrix

| Task Type | Primary Mode | Secondary Mode | Cost | Quality Impact |
|-----------|-------------|----------------|------|----------------|
| **Landing page hero** | Standard + Mythos | — | €0.15 | Punchy + structured |
| **Case study** | **Fable** | Deep Research (for stats) | €0.30 | Emotion + evidence |
| **Founder story** | **Fable** | — | €0.15 | Emotional connection |
| **Methodology page** | **Mythos** | — | €0.20 | IP differentiation |
| **Industry benchmarks** | **Deep Research** | Standard (for summary) | €0.40 | Credibility |
| **Blog post (how-to)** | Standard | — | €0.10 | Functional |
| **Blog post (thought leadership)** | **Deep Research** + Mythos | — | €0.50 | Authority |
| **Email sequence** | **Fable** | Standard (for CTAs) | €0.25 | Nurture + convert |
| **FAQ** | Standard | — | €0.10 | Clear answers |
| **About page** | **Fable** | Mythos (for framework) | €0.25 | Story + system |

---

## Integration with Build Orchestrator

### Where This Fits

```
[Design Spec Complete]
   ↓
[Copy Agent Assigns Claude Modes]
   ├──(Hero, CTAs)──▶ Standard mode
   ├──(Case studies)──▶ Fable mode
   ├──(Methodology)──▶ Mythos mode
   ├──(Benchmarks)──▶ Deep Research mode
   └──(Blog content)──▶ Mode varies by post type
   ↓
[Quality Gate: Does copy match assigned mode?]
   ↓
[Build]
```

### Cost Estimation with Modes

| Build Tier | Modes Used | Est. Cost | Output |
|-----------|-----------|-----------|--------|
| **Essential** | Standard only | €0.50-1.00 | Functional, competent |
| **Professional** | Standard + Fable | €1.00-1.50 | Good stories, solid structure |
| **Portfolio** | **All four modes** | €1.50-3.00 | Memorable, differentiated, cited |

---

## Anti-Patterns

### Anti-Pattern 1: Using Standard for Everything

**Symptom:** All copy sounds the same. No emotional resonance. No differentiation.
**Fix:** Assign Fable to case studies. Assign Mythos to methodology.

### Anti-Pattern 2: Using Fable for Data

**Symptom:** Beautiful story, zero evidence. "Trust me, it works."
**Fix:** Pair Fable with Deep Research. Story + stats.

### Anti-Pattern 3: Using Deep Research Without Story

**Symptom:** Wall of stats. Boring. No emotional hook.
**Fix:** Lead with Fable (story), support with Deep Research (stats).

### Anti-Pattern 4: Using Mythos for Simple Content

**Symptom:** Over-engineered explanation for a simple concept.
**Fix:** Mythos for systems. Standard for simple descriptions.

---

## Example: AI Revenue Systems Site Content Plan

| Section | Claude Mode | Why | Cost |
|---------|------------|-----|------|
| **Hero headline** | Standard | Punchy, specific | €0.05 |
| **Hero subhead** | Standard | Clear value prop | €0.05 |
| **Social proof marquee** | Standard | Quick facts | €0.05 |
| **Problem section** | **Deep Research** | Cited pain points | €0.20 |
| **Audit offer** | Standard | Clear features/pricing | €0.10 |
| **Case study 1** | **Fable** + Deep Research | Story + stats | €0.30 |
| **Case study 2** | **Fable** + Deep Research | Story + stats | €0.30 |
| **Case study 3** | **Fable** + Deep Research | Story + stats | €0.30 |
| **FAQ** | Standard | Clear answers | €0.10 |
| **Final CTA** | Standard | Direct conversion | €0.05 |
| **About/Founder** | **Fable** | Emotional connection | €0.15 |
| **Methodology page** | **Mythos** | System framework | €0.20 |
| **Total content cost** | | | **€1.85** |

---

## How to Invoke Each Mode in Practice

### Subagent Prompt Template

```
You are operating in [MODE] mode.

[If Fable:]
"Write this as a compelling narrative. Use scene-setting, 
character emotion, and a story arc. The reader should feel 
the transformation."

[If Mythos:]
"Explain this as an interconnected system. Show how the 
parts relate, the inputs and outputs, the feedback loops. 
This should feel like a teachable framework."

[If Deep Research:]
"Research this thoroughly. Use web search. Cite sources. 
Provide data and statistics. Flag unverified claims."

[If Standard:]
"Write clear, concise, accurate content. Be specific. 
Avoid generic claims."

Task: [specific task]
Output: [expected format]
```

---

*Skill created: 2026-08-02*  
*Source: User research + Anthropic documentation + Session learnings*
