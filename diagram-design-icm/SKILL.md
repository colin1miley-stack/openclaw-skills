---
name: diagram-design-icm
description: Editorial diagram generation for Signal content and Revenue Systems deliverables. ICM-converted from cathrynlavery/diagram-design. Creates branded architecture, flowchart, funnel, timeline, quadrant, and 34 other diagram types as self-contained HTML/SVG.
---

# Diagram Design — ICM Converted

**Source:** `cathrynlavery/diagram-design` (MIT License)  
**Converted:** 2026-09-05  
**Purpose:** Visual content generation for Signal posts, Revenue Systems proposals, audit deliverables  

---

## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| **Layer 0** | `SOUL.md`, `AGENTS.md` | Brand consistency, no default tokens |
| **Layer 1** | Root `CONTEXT.md` | Route to content/visual stages |
| **Layer 2** | This SKILL.md | Task contract for diagram generation |
| **Layer 3** | `skills/diagram-design-icm/`, `references/` | Style guide, semantic patterns |
| **Layer 4** | `stages/07-content-visuals/output/` | Generated diagrams, brand profiles |

---

## What Diagram Design Does

```
Input: Concept, data, or system description
    ↓
Semantic pattern selection (behavior → layout)
    ↓
Visual type selection (39 types)
    ↓
Brand token application (colors, typography)
    ↓
Self-contained HTML/SVG output
    ↓
Signal: LinkedIn carousel, blog header, proposal figure
```

**39 Visual Types:**
- **System:** Architecture, IT current-state, data flow, process
- **Decision:** Flowchart, state machine, sequence, swimlane
- **Data:** Bar, line, scatter, radar, polar, treemap, Gantt
- **Structure:** Tree, org chart, nested, layer stack, Venn
- **Strategy:** Quadrant, pyramid/funnel, timeline, loop/flywheel
- **New (2.5.10):** Sankey, fishbone, Wardley map, kanban, user journey, deployment, dependency graph, UML class, story map, database schema

---

## Brand Integration for Colin Miley

### Revenue Systems Brand Tokens

```yaml
# .diagram-design profile: revenue-systems
paper: "#ffffff"
ink: "#1a1a2e"           # Deep navy
accent: "#D4A574"         # Warm gold
coral: "#0f766e"          # Emerald
secondary: "#4a5568"
font_heading: "Inter Bold"
font_body: "Inter Regular"
```

### Signal Content Diagrams

| Content Type | Diagram Type | Example |
|-------------|--------------|---------|
| Revenue leak audit | **Funnel** | "Where leads drop off" |
| Content engine | **Loop/Flywheel** | "Signal → Content → Leads → Revenue" |
| Platform strategy | **Quadrant** | "Generation vs Distribution" |
| Process explanation | **Architecture** | "Signal ecosystem" |
| Competitive analysis | **Radar** | "vs competitors" |
| Timeline | **Timeline** | "Roadmap to launch" |

---

## Signal Integration

### Generate Diagram for LinkedIn Post

```python
# Signal content pipeline addition
async def generate_diagram_content(brief: ContentBrief) -> ContentPiece:
    # Select diagram type based on content pillar
    diagram_type = brief.pillar.diagram_type  # e.g., "loop" for flywheel
    
    # Generate branded diagram
    diagram_html = await diagram_design.generate(
        type=diagram_type,
        data=brief.data,
        brand_profile="revenue-systems",
        size="linkedin-carousel",  # 1080x1080
    )
    
    # Convert to PNG for social
    diagram_png = await convert_to_png(diagram_html)
    
    return ContentPiece(
        text=brief.text,
        media=[diagram_png],
        platform=Platform.LINKEDIN,
    )
```

---

## ICM Stage Contract

```json
{
  "stageName": "content-visuals",
  "branchName": "feat/diagram-design",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "cv-001",
      "title": "Create brand profile for Revenue Systems",
      "agent": "builder",
      "acceptance": ["Tokens extracted from colinmiley.com", "Saved as named profile"],
      "estimatedTokens": 5000
    },
    {
      "id": "cv-002",
      "title": "Create Signal ecosystem architecture diagram",
      "agent": "builder",
      "acceptance": ["All components shown", "Brand colors applied", "SVG output"],
      "estimatedTokens": 8000
    },
    {
      "id": "cv-003",
      "title": "Create content flywheel diagram",
      "agent": "builder",
      "acceptance": ["Loop format", "Signal → Leads → Revenue cycle", "LinkedIn-ready size"],
      "estimatedTokens": 8000
    },
    {
      "id": "cv-004",
      "title": "Create competitive quadrant",
      "agent": "builder",
      "acceptance": ["Generation vs Distribution axes", "Competitors positioned", "Brand accent"],
      "estimatedTokens": 6000
    }
  ]
}
```

---

## Quality Gate

- [ ] Brand profile matches colinmiley.com colors
- [ ] No default atomic-tangerine accent
- [ ] Target density: 4/10 (not overcrowded)
- [ ] 1-2 focal nodes with coral accent
- [ ] Output is self-contained HTML/SVG
- [ ] Renders correctly in browser
- [ ] Optimized for target platform size

---

*ICM-converted from cathrynlavery/diagram-design. See original repo for full type reference.*
