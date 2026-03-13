---
name: extract-skill
description: Extracts specialized knowledge from web pages or files to create or update Agent Skills. Use this when you find a technical document, README, or API guide and want to turn it into a reusable capability in your skills library.
license: Apache-2.0
metadata:
  version: 0.0.0
  author: trebaud
---

# Extract Skill Instructions

This skill enables you to automatically grow your capabilities by parsing documentation and transforming it into the standard Agent Skills format.

## When to Use
- When you encounter a URL containing a guide, tutorial, or API reference.
- When you are given a local file (PDF, Markdown, Text, JSON) describing a specific workflow.
- When you want to formalize a discovered pattern into a permanent skill within a chat session

## Information Filtering Principles

**CRITICAL**: Extract only high-value, high-density information using the epistemic distillation heuristics defined in [references/distillation-heuristics.md](references/distillation-heuristics.md).

### Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or scripts with parameters)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

Think of an AI agent as exploring a path: a narrow bridge with cliffs needs specific guardrails (low freedom), while an open field allows many routes (high freedom).

## Structured Skill Creation Process

### Step 1: Understanding with Concrete Examples

Before extracting, understand concrete examples of how the skill will be used:

- **Identify workflows**: What specific tasks will users perform?
- **Map trigger patterns**: What phrases should activate this skill?
- **Define scope**: What functionality is essential vs. optional?

**Example**: When processing a PDF editing guide, ask:
- "What types of PDF operations are most common?"
- "Are there specific PDF formats or use cases to focus on?"
- "What would a user say that should trigger this skill?"

### Step 2: Ingest Source Content

- **For URLs**: Use `curl` or a browsing tool to fetch the full content of the page.
- **For Files**: Read the file content directly.
- **Analysis**: Identify the core capability, its name (kebab-case), and its primary use case.
- **Filter**: Apply epistemic distillation heuristics from [references/distillation-heuristics.md](references/distillation-heuristics.md) to extract only high-value, high-density content.

### Step 3: Context Comparison (Deduplication)

- List the existing skills in your skills directory (typically, `.opencode/skills/`, `.claude/skills/`, or `~/.agents/skills/`).
- Compare the new capability against existing ones:
    - **Orthogonal**: If the functionality is unrelated to any existing skill, proceed to **Create**.
    - **Overlapping**: If a skill already exists for this domain, proceed to **Update**.

### Step 4: Planning Reusable Resources

Analyze each concrete example to identify what resources would be helpful:

**Example Analysis**:
- **PDF rotation skill**: Users repeatedly need the same rotation code → Create `scripts/rotate_pdf.py`
- **Frontend builder skill**: Users need the same boilerplate → Create `assets/frontend-template/`
- **API integration skill**: Users need schema information → Create `references/schemas.md`

**Resource Decision Framework**:
- **Scripts needed?** → Same code rewritten repeatedly or deterministic reliability required
- **References needed?** → Complex documentation, domain knowledge, or multiple variants
- **Assets needed?** → Files used in output (templates, images, boilerplate)

### Step 5: Create and Implement

**Reference Standards**: Before implementation, read [references/skill-creation-standards.md](references/skill-creation-standards.md) for structural requirements and [references/distillation-heuristics.md](references/distillation-heuristics.md) for content filtering principles.

### Step 6: Validate and Iterate

After creating the skill structure and content, run the validation script:
```bash
scripts/validate-skill ./skill-name
```

## Updating/Merging a Skill
- Read the existing `SKILL.md`
- Append new instructions or examples to the body
- Update the `description` if scope expanded significantly
- Ensure no duplicate frontmatter sections are created

## Examples and Edge Cases

### Input/Output Examples
**Example Input**: URL to API documentation
```
https://api.example.com/docs/authentication
```
**Expected Output**: 
- Skill directory: `api-authentication/`
- SKILL.md with frontmatter and condensed OAuth implementation guide
- scripts/generate-auth-middleware.js (if repetitive patterns found)

**Example Input**: Local PDF with workflow guide
```
./data-workflow-guide.pdf
```
**Expected Output**:
- Skill directory: `data-processing-workflow/`
- SKILL.md with ETL patterns and error handling
- references/schemas.md (if multiple data formats)

<example>
**Source Document**: "5,000-word blog post about debugging Node.js applications with extensive background, personal anecdotes, and basic explanations."
**Action**: Extract only the debugging patterns and create `nodejs-debugging-patterns`.
**Resulting Description**: "Applies systematic debugging patterns for Node.js applications. Use when you need to identify and resolve performance issues, memory leaks, or runtime errors efficiently."
**Note**: Reduced from 5,000 words to ~200 words while preserving 100% of actionable debugging techniques.
</example>

<example>
**Source Document**: "3,000-word OAuth 2.0 tutorial including history of authentication, basic web concepts, and marketing language about security benefits."
**Action**: Extract only the implementation patterns and create `oauth2-nodejs-implementation`.
**Resulting Description**: "Implements OAuth 2.0 authentication flows in Node.js applications using Express. Use when you need to add secure user authentication and authorization to web APIs."
**Note**: Filtered out 1,800 words of non-essential content, keeping only code patterns and flow logic.
**Enhanced with Scripts**: Added `scripts/generate-oauth-setup.js` for boilerplate middleware creation, `scripts/keygen.sh` for secure key generation, and `assets/providers/` with templates for Google, GitHub, and custom OAuth providers.
</example>

<example>
**Source Document**: "PDF research paper on advanced machine learning model optimization techniques."
**Action**: Create `ml-model-optimization`.
**Resulting Description**: "Applies cutting-edge optimization techniques to improve machine learning model performance and efficiency. Use when you need to enhance model accuracy, reduce training time, or optimize resource usage."
</example>

<example>
**Source Document**: "Linux command reference for ollama AI model management and deployment."
**Action**: Create `ollama-linux-management`.
**Resulting Description**: "Manages and deploys AI models using ollama on Linux systems. Use when you need to run, configure, or interact with local LLM models via command line."
**Enhanced with Scripts**: Added `scripts/quick-setup.sh` for automated environment preparation, `scripts/model-monitor.sh` for resource monitoring, and `assets/config-templates/` with pre-built Modelfile templates for common use cases.
</example>

### Common Edge Cases
- **Multiple capabilities**: When source contains distinct functionalities, create separate skills or use domain-specific organization
- **Incomplete documentation**: Fill gaps with best practices, clearly mark assumptions
- **Outdated information**: Extract timeless patterns, note version-specific details
- **Mixed media content**: Focus on textual instructions, ignore video/image references
