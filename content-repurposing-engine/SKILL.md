<!--
License: CC BY-NC-SA 4.0 (https://creativecommons.org/licenses/by-nc-sa/4.0/)
Copyright (c) 2026 Colin Miley � AI Revenue Systems
Commercial use requires permission: colin@colinmiley.com
-->
---
name: content-repurposing-engine
description: |
  Transform one piece of source content (topic, URL, blog post, newsletter, or raw text) into a complete
  multi-platform content package: platform-native drafts + Nano Banana visual prompts + scheduling metadata.
  Use when the user wants to "repurpose content," "create social posts from a blog," "turn a newsletter into
  LinkedIn posts," or "generate a week's worth of content from one idea."
  
  Originally built for AI Revenue Systems. Adaptable to any B2B solopreneur/consultant brand.
metadata:
  version: 1.0.0
  originally_built_for: AI Revenue Systems / Colin Miley
  brand_palette:
    primary: "#0F172A"
    accent: "#FF6B6B"
    background: "#FAFAF9"
    text: "#1E293B"
    secondary_accent: "#84A98C"
---

# Content Repurposing Engine

Transform one input into a full multi-platform content package. Built for speed, brand consistency, and
platform-native formatting.

---

## Input

Accept any of:
1. **Topic** (e.g., "How solopreneurs use AI to replace busywork")
2. **URL** (blog post, newsletter, article — will be fetched and summarized)
3. **Raw text** (newsletter draft, rough notes, transcript excerpt)
4. **Existing post** (e.g., "Turn my LinkedIn post into X + Instagram variants")

## Output Format

For each platform, produce:
- **Draft text** — platform-native format, character limits respected, hooks optimized
- **Visual prompt** — ready to paste into Nano Banana
- **Scheduling metadata** — best time to post, optional caption/hashtags

---

## Platform Specifications

### LinkedIn
- **Format:** Long-form thought leadership, 150-300 words, 3-5 short paragraphs
- **Hook:** Open with a bold claim, counterintuitive take, or specific number
- **Body:** Story → Insight → Actionable takeaway
- **CTA:** Question to drive comments, or soft CTA to newsletter/consulting
- **Hashtags:** 3-5 max, relevant and specific (not #business #success)
- **Visual:** Carousel (3-5 slides) or single infographic-style image
- **Best time:** Tuesday-Thursday, 8-10 AM GMT

### X / Twitter
- **Format:** Thread (3-7 tweets) or single stand-alone tweet
- **Hook:** Tweet 1 must stop the scroll. Use specificity, contradiction, or curiosity gap
- **Thread flow:** Hook → Expand → Proof → Insight → CTA
- **Character limit:** 280 per tweet (respect hard limit)
- **Visual:** Single image or carousel attached to Tweet 1
- **Best time:** Tuesday-Thursday, 12-2 PM GMT

### Instagram
- **Format:** Carousel post (5-10 slides) OR single image with caption
- **Hook (slide 1):** Bold statement or question, minimal text, high contrast
- **Body slides:** One idea per slide, numbered, progressive revelation
- **Caption:** Short hook + 2-3 sentences context + CTA + 5-8 hashtags
- **Visual:** High-contrast, readable at small size, brand colors
- **Best time:** Monday/Wednesday/Friday, 6-8 PM GMT

### YouTube (Shorts / Community Post)
- **Format:** Script for 60-second Short OR community post text
- **Hook (first 3 seconds):** Visual + verbal hook that stops the scroll
- **Script structure:** Hook → Problem → Solution → Proof → CTA
- **Visual prompt:** Thumbnail concept (1280x720, high contrast, readable text)
- **Best time:** Saturday/Sunday, 10 AM-2 PM GMT

---

## Brand Voice Guardrails

Apply these to EVERY output:

1. **Tone:** Sharp, practical, no corporate speak. 15y SaaS sales experience comes through.
2. **No AI slop:** No "In today's fast-paced world..." No "Let's dive in..." No emoji overload.
3. **Specificity over abstraction:** Real numbers, real examples, real stories. "I lost £3K on a bad hire" > "Hiring is hard."
4. **Contrarian where earned:** Challenge conventional wisdom, but back it up with experience.
5. **CTA discipline:** Every post has ONE clear next step. No "follow for more" fluff.
6. **Fact base alignment:** All claims verifiable against COLIN-FACT-BASE.md. No invented stats.

---

## Visual Prompt Engineering for Nano Banana

Every visual prompt must include:
- **Dimensions** (platform-specific)
- **Color palette** (brand colors, hex codes)
- **Style** (minimal, professional, dark theme where appropriate)
- **Text overlay** (exact text to render, if any — Nano Banana handles text well)
- **Composition** (layout, focal point, negative space)

### Visual Prompt Template

```
Create a [platform] [format] image.

Dimensions: [width x height]
Style: Minimal, professional, dark navy background (#0F172A) with warm coral (#FF6B6B) accents.
Typography: Clean sans-serif, white text (#FAFAF9), high contrast.
Composition: [specific layout]
Text overlay: "[exact text]"
Additional elements: [icons, shapes, texture details]
Mood: [authoritative / approachable / energetic / calm]
```

---

## Execution Steps

### Step 1: Understand the Source
- If URL: Fetch, summarize key points, extract 3-5 core ideas
- If topic: Research briefly, identify the strongest angle
- If raw text: Identify the central thesis and 2-3 supporting points
- If existing post: Deconstruct hook, structure, and core message

### Step 2: Extract Core Ideas
Distill into 3-5 atomic ideas. Each idea should be:
- Standalone (makes sense without the others)
- Specific (not generic advice)
- Visualizable (can be turned into an image)

### Step 3: Map Ideas to Platforms
- **LinkedIn:** Best idea → long-form post (full argument)
- **X:** Best idea → thread (telescoped version) OR second-best idea → single tweet
- **Instagram:** Most visual idea → carousel OR most contrarian → single image
- **YouTube:** Most story-driven idea → Short script

### Step 4: Draft Platform Copy
Write each draft respecting platform format, character limits, and brand voice.

### Step 5: Generate Visual Prompts
For each platform draft, create a Nano Banana prompt that:
- Matches the post's core message
- Uses brand colors
- Is optimized for that platform's dimensions
- Includes legible text overlay where appropriate

### Step 6: Output Package
Present everything in a structured format (see below).

---

## Output Template

```markdown
# Content Package: [Topic / Source Title]
Source: [URL or "Original topic"]
Date: [YYYY-MM-DD]

---

## Core Ideas Extracted

1. [Idea 1 — one sentence]
2. [Idea 2 — one sentence]
3. [Idea 3 — one sentence]

---

## LinkedIn Post

**Draft:**
[Full text, 150-300 words, formatted with line breaks]

**Visual Prompt (Nano Banana):**
[Ready-to-paste prompt]

**Hashtags:** #[tag1] #[tag2] #[tag3]
**Best time to post:** [Day, Time GMT]
**CTA target:** [newsletter signup / consulting call / comment engagement]

---

## X / Twitter Thread

**Tweet 1 (Hook):**
[≤280 chars, scroll-stopping]

**Tweet 2:**
[≤280 chars]

**Tweet 3:**
[≤280 chars]

**Tweet 4:**
[≤280 chars]

**Tweet 5 (CTA):**
[≤280 chars]

**Visual Prompt (Nano Banana):**
[Ready-to-paste prompt for Tweet 1 image]

**Best time to post:** [Day, Time GMT]

---

## Instagram Post

**Slide 1 (Cover):**
[Text for cover slide — bold, minimal]

**Slide 2:**
[Text for slide 2]

**Slide 3:**
[Text for slide 3]

**Caption:**
[Full caption text, 2-3 sentences + CTA + hashtags]

**Visual Prompts (Nano Banana — one per slide):**
1. [Prompt for slide 1]
2. [Prompt for slide 2]
3. [Prompt for slide 3]

**Hashtags:** #[tag1] #[tag2] #[tag3] #[tag4] #[tag5]
**Best time to post:** [Day, Time GMT]

---

## YouTube Short Script

**Title:** [60 chars max, keyword-rich]

**Hook (0-3 sec):**
[What the viewer sees and hears]

**Script:**
[0:00-0:10] [Line 1]
[0:10-0:20] [Line 2]
[0:20-0:30] [Line 3]
[0:30-0:45] [Line 4]
[0:45-0:60] [CTA]

**Visual Prompt (Thumbnail):**
[Ready-to-paste prompt for 1280x720 thumbnail]

**Best time to post:** [Day, Time GMT]

---

## Scheduling Queue

| Platform | Content | Day | Time GMT | Status |
|----------|---------|-----|----------|--------|
| LinkedIn | [Idea 1] | Tue | 8:30 AM | Draft ready |
| X | [Idea 2] | Wed | 12:00 PM | Draft ready |
| Instagram | [Idea 3] | Fri | 7:00 PM | Draft ready |
| YouTube | [Idea 1 variant] | Sat | 11:00 AM | Draft ready |
```

---

## Quality Checklist

Before delivering the package, verify:

- [ ] Every post has ONE clear CTA
- [ ] No generic AI filler language
- [ ] All claims are specific or backed by COLIN-FACT-BASE.md
- [ ] Character limits respected (X: 280, LinkedIn: 3,000 but aim for 300)
- [ ] Visual prompts include exact dimensions and hex colors
- [ ] Hashtags are specific, not generic
- [ ] Hook on every platform is scroll-stopping
- [ ] Brand voice is consistent across all platforms

---

## Example: Live Test Topic

**Input:** "How solopreneurs use AI to replace busywork"

**Core Ideas:**
1. AI doesn't replace strategy — it replaces the 2-hour task that keeps you from strategizing
2. The 21/month tool stack that runs my entire content operation
3. Why hiring a VA before building systems is burning money

**LinkedIn Draft Preview:**
> I spent 3,000 on a VA before I built a single system.
>
> She was great. But she was manually doing things that AI could handle in 30 seconds.
>
> Three months in, I realised: I hadn't bought time. I'd bought a slower version of myself.
>
> Here's what I should have done first...

**Nano Banana Prompt Preview:**
> Create a LinkedIn carousel cover slide. Dimensions: 1080x1350. Dark navy background (#0F172A) with warm coral accent (#FF6B6B). Clean sans-serif typography in white. Text overlay: "I spent 3,000 on a VA before I built a single system." Minimal layout, high contrast, professional mood.

---

## Related Skills

| When to hand off | Skill |
|------------------|-------|
| Need market research on what topics perform | `market-research-for-brand` |
| Need copywriting polish / humanizer gate | `ai-voice-humanizer` |
| Need landing page for content service | `landing-page-generator` |
| Need to set up analytics tracking | `analytics-tracking` |
| Need client onboarding SOPs | `gtm-strategy` |
