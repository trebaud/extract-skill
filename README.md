# Extract Skill

A meta Agent Skill that enables automated capability growth by extracting specialized knowledge from documentation and transforming it into standardized Agent Skills.

## Overview

`extract-skill` follows the [Agent Skills specification](https://agentskills.io/specification) and is inspired by Anthropic's [skill creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) to parse technical documents, README files, API guides, and tutorials into concise, reusable skills. It applies aggressive information filtering to extract only high-value, information dense content.

## When to Use

- Converting URLs with guides, tutorials, or API references into reusable skills
- Processing local files (PDF, Markdown, Text, JSON) describing specific workflows  
- Formalizing discovered patterns into permanent capabilities within chat sessions

## Key Features

- **Context Window Optimization**: Prioritizes essential information with maximum value per word
- **Progressive Disclosure**: Three-level loading system (metadata → SKILL.md → bundled resources) for efficient context management
- **Information Filtering**: Applies epistemic distillation heuristics to extract only high-value, high-density information (50-70% reduction from source)
- **Deduplication**: Compares against existing skills to avoid overlap
- **Degrees of Freedom**: Matches specificity to task variability (high/medium/low freedom patterns)
- **Standardization**: Follows Agent Skill Specification for consistent formatting
- **Structured Resources**: Strategic use of scripts, references, and assets based on use case requirements
- **Quality Control**: Ensures maximum actionable insight per word with no redundancy

## Workflow

1. **Understand**: Analyze concrete examples and define scope with user input
2. **Ingest**: Fetch content from URLs or read local files
3. **Filter**: Apply epistemic distillation heuristics from reference documents to extract essential patterns
4. **Compare**: Check against existing skills for deduplication
5. **Plan**: Identify necessary scripts, references, and assets
6. **Create**: Generate new skills with progressive disclosure structure
7. **Resource Strategy**: Implement appropriate bundled resources based on use case
8. **Validate**: Run `scripts/validate-skill ./skill-name` to ensure quality standards compliance

## Reference Documents

### Epistemic Distillation Heuristics
The `references/distillation-heuristics.md` document defines universal principles for extracting high-density, actionable knowledge:

- **Information Theory**: Signal vs. noise separation using Shannon entropy
- **Gricean Maxims**: Linguistic economy and precise communication
- **Epistemic Filters**: Truth and utility validation through Occam's Razor and Pareto Principle
- **Semiotic Abstraction**: Pattern extraction and structural isomorphism identification
- **Teleological Validation**: Actionability and future utility assessment

**Goal**: Provide a systematic framework for extracting only the most valuable insights from any source material while eliminating redundant or low-value content.

## Quality Standards

- `lowercase-hyphenated-names` (max 64 characters)
- Context-optimized descriptions with discovery keywords
- Progressive disclosure structure (metadata → SKILL.md → references)
- Degrees of freedom matching (high/medium/low specificity)
- Strategic resource allocation (scripts for reliability, references for knowledge, assets for output)
- Epistemic distillation applied to all content extraction
- No extraneous documentation files
- Action-oriented instructions focused on execution

Transforms verbose documentation into precise, context-efficient capabilities with appropriate automation while preserving 100% of essential techniques.
