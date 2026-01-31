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

#### File output rules
- Before writing, inspect the project directory structure (`ls` root and relevant subdirectories)
- Follow the existing directory organization conventions
- Place research files under `research/{topic}/` subdirectory (e.g. `research/clawdbot/`)
- Multiple documents about the same topic MUST live in the same subdirectory
- Keep naming and grouping consistent with sibling directories

#### Determine deliverables
- For deployable / installable / usable technologies, produce TWO documents:
  1. **Overview report** — what it is, why it matters, pros/cons analysis
  2. **Hands-on guide** — how to deploy, configure, and use it (full step-by-step)
- For pure concepts / theories / comparisons, a single overview report is sufficient
- If the user's request contains specific questions, answer them directly at the top of the report (e.g. a "Key Questions Answered" section)

#### Overview report structure

```
# {Topic} Deep Research Report
> Date: {{date}}

## TL;DR (one paragraph)
## Key Questions Answered (if user asked specific questions)
## Core Concepts
## How It Works (with diagrams if helpful)
## Framework/Tool Comparison (table)
## Latest Developments (2025-2026)
## When to Use / When NOT to Use
## Getting Started (beginner path)
## Sources
```

#### Hands-on guide structure rules
- Organize by **user workflow**, NOT by knowledge category
- If there are multiple deployment methods / approaches, each MUST be a **complete, self-contained pipeline** from start to finish
- Readers should pick one approach and follow it top-to-bottom without jumping to other sections
- Shared configuration (e.g. API options) MUST be repeated inside each approach — do NOT extract into a separate chapter that forces readers to jump around
- Each step should include the exact commands, config snippets, and expected output
- Clearly label which approach each instruction belongs to (e.g. "A-3", "B-2")

### Quality rules
- All comparisons: at least cover positioning, pros/cons, use cases, community activity
- Latest section MUST include live search results, not just training knowledge
- Include source URLs for all factual claims
- Explicitly mark if any info might be outdated
- Output in Chinese (Simplified), code/terms keep English
