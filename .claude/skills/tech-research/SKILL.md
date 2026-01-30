---
name: tech-research
description: |
  Deep research on a specific technology topic. Trigger when user says
  "调研", "research", "深入了解", or asks to compare technologies.
  Searches latest info, cross-references sources, outputs structured report.
allowed-tools:
  - Bash
  - Read
  - Write
---

# Technology Deep Research

Research topic: {{arguments}}

## Process

### Step 1: Clarify scope
- What is this technology?
- What problem does it solve?
- What are the key sub-topics to cover?

### Step 2: Search latest information
Use multiple sources (MUST search, do NOT rely solely on training data):
- Web search for latest articles, blogs, news (focus on 2025-2026)
- DeepWiki for key GitHub projects related to the topic
- Context7 for framework/library documentation
- Cross-reference at least 3 sources per major claim

### Step 3: Analyze and synthesize
- Core concepts and how they work
- Compare top implementations/frameworks (table format)
- Current trends and future direction
- Limitations, criticisms, and open problems

### Step 4: Write report
Save as markdown file in current directory. Use this structure:

```
# {Topic} Deep Research Report
> Date: {{date}}

## TL;DR (one paragraph)
## Core Concepts
## How It Works (with diagrams if helpful)
## Framework/Tool Comparison (table)
## Latest Developments (2025-2026)
## When to Use / When NOT to Use
## Getting Started (beginner path)
## Sources
```

### Quality rules
- All comparisons: at least cover positioning, pros/cons, use cases, community activity
- Latest section MUST include live search results, not just training knowledge
- Include source URLs for all factual claims
- Explicitly mark if any info might be outdated
- Output in Chinese (Simplified), code/terms keep English
