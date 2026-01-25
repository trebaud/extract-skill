---
name: software-estimation
description: Applies pragmatic software work estimation strategies that account for organizational politics and unknown variables. Use when you need to estimate project timelines and communicate risks to stakeholders.
---

# Software Estimation

Estimates are political tools for managers, not engineering delivery mechanisms. Your job is to find technical approaches that match the expected timeline, not determine how long work will take.

## When to Use
- When asked to provide timeline estimates for software projects
- When planning work with management stakeholders
- When negotiating project scope vs timeline constraints

## Core Principles

### Estimates Flow Downward
- Management already has a timeline in mind (even if unstated)
- Your job: find technical approaches that fit their timeline
- Never start by asking "how long will this take"
- Always ask "what approaches fit in [timeline]"

### Unknown Work Dominates
- 90% of project time is spent on unknown problems
- Only known work can be accurately estimated
- Focus risk assessment on unknown variables, not known tasks
- More "dark forests" in codebase = higher risk/longer timeline

### Never Give Single Numbers
- Always return a range of options with different risk profiles
- Present multiple approaches: conservative, moderate, aggressive
- Let management choose the risk/reward tradeoff
- Build trust by being pragmatic, not by always pushing back

## Estimation Workflow

### 1. Gather Political Context First
Before looking at code:
- How much pressure is on this project?
- Is this a "must do" or "nice to have"?
- What timeline is management expecting?
- Who are the stakeholders and what do they care about?

### 2. Approach with Timeline in Hand
- Start with the expected timeline, not the technical solution
- Ask: "What can we build in [timeline]?"
- Constraint drives technical decisions, not the other way around

### 3. Assess Unknown Variables
- Identify all "dark forests" the feature must touch
- Rate each unknown by scariness/risk level
- Focus estimate on unknowns, not known implementation
- More unknowns = need tighter constraints on approach

### 4. Return Risk Assessment
Present multiple options:
- **Option A**: Full implementation (higher risk, longer timeline)
- **Option B**: Simplified approach (reduced scope, meets timeline)
- **Option C**: Alternative solution (different risks, different timeline)

## Communication Patterns

### Good: Risk-Based Response
"I don't think we'll hit one week because X, Y, and Z all need to go perfectly, and at least one will take longer than expected. Here are three approaches..."

### Bad: Concrete Estimate
"This will take four weeks."

### Good: Multiple Options
- We can tackle X, Y, Z directly (might work, could be a month)
- We can bypass Y and Z (different risks, might hit deadline)
- We can get help from team familiar with X (we focus on Z only)

### Bad: Single Approach
"We need to build X, Y, and Z properly."

## Managing Impossible Projects
- Only claim "impossible" when genuinely technically infeasible
- Build trust by being pragmatic on other estimates
- When impossible, management must alter requirements
- Always pushing back reduces credibility when you need it

## Notes
- Estimates are for managerial negotiation, not engineering accuracy
- Your value is finding workable solutions within constraints
- The tighter the timeline, the more creative the technical approach must be