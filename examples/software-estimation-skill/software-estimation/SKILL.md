---
name: software-estimation
description: Applies pragmatic software work estimation strategies that account for organizational politics and unknown variables. Use when you need to estimate project timelines and communicate risks to stakeholders in corporate environments.
license: Apache-2.0
metadata:
  version: 0.0.0
  author: trebaud
---

# Software Estimation

## When to Use
- When asked to provide time estimates for software projects
- When management pressure influences estimation process
- When working with poorly-understood systems or large codebases
- When you need to communicate technical constraints to non-technical stakeholders

## Core Principles

**Estimates are political tools, not engineering predictions**
- Management already has timeline expectations before talking to you
- Your job is to find approaches that fit their timeline, not predict how long work will take
- Never provide concrete estimates - always offer risk assessments and options

**Unknown work dominates projects (90% of time)**
- Only well-understood, small-scope work can be accurately estimated
- Most programming in large systems is research and discovery
- Focus on identifying "dark forests" in codebase that create uncertainty

## Estimation Workflow

### 1. Gather Political Context First
- Ask: How much pressure is on this project?
- Determine: Is this a casual ask or a mandatory deadline?
- Identify: What timeline is management expecting?
- Understand: Who are the stakeholders and what do they want?

### 2. Start with Target Timeline
- Begin with the estimate already in hand from management
- Ask: "Which approaches could be done in one week?" instead of "How long will this take?"
- Constrain technical solutions to fit timeline expectations

### 3. Assess Unknowns vs Knowns
- Identify all "dark forests" (uncertain areas in codebase)
- Rate each unknown by risk level and potential time impact
- Focus more time on unknowns than on known work

### 4. Return with Options, Not Estimates
Never say "this is a four-week project". Instead provide:
- Direct approach with risks (might work, but could blow out to a month)
- Alternative approaches bypassing risky components
- Options involving help from other teams
- Each option with different risk/reward tradeoffs

## Communication Strategy

**Use risk-based language:**
- "I don't think we'll get this done in one week because X, Y, Z would need to all go right"
- "At least one of those things is bound to take more work than we expect"

**Provide multiple paths:**
- Technical approach with highest risk
- Simplified approach with moderate risk  
- Collaborative approach with external dependencies

**Build trust through pragmatism:**
- Occasionally push back when projects are genuinely impossible
- Don't always say no to pressure - make pragmatic compromises
- Build credibility so you're believed when something truly can't be done

## When Projects Are Impossible

- Signal impossibility only when absolutely necessary
- Requires pre-built trust with management
- Provide clear technical reasons why no approach fits timeline
- Suggest requirement changes rather than just saying "no"

## Common Anti-Patterns to Avoid

- Don't insist on answering all unknown questions before estimating
- Don't refuse to estimate under uncertainty
- Don't push back against every management request
- Don't treat estimation as pure technical exercise

## Team Context Considerations

- Team capability dramatically impacts estimates (10x differences between teams)
- Some teams cannot deliver work at all regardless of timeline
- Company expectations often assume estimates are fungible between engineers
- Consider actual team velocity when assessing feasibility