---
name: extract-skill
description: Extracts specialized knowledge from web pages or files to create or update Agent Skills. Use this when you find a technical document, README, or API guide and want to turn it into a reusable capability in your skills library.
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

**CRITICAL**: Extract only high-value, high-density information. Your goal is to create concise, actionable skills—not comprehensive documentation repositories.

- **High-Value**: Information that directly enables a specific capability or solves a recurring problem
- **High-Density**: Content with maximum actionable insight per word, avoiding fluff, marketing copy, or verbose explanations
- **Essential Only**: Include only what's necessary to understand and execute the core capability
- **Pattern-Focused**: Prioritize reusable patterns, workflows, and technical approaches over one-off examples

**What to EXCLUDE**:
- Marketing language, product descriptions, or promotional content
- Basic explanations or introductory material
- Repetitive examples that don't add new patterns
- Historical context or background information
- Anything that doesn't directly enable the core capability

## Workflow

### 1. Ingest Source Content
- **For URLs**: Use `curl` or a browsing tool to fetch the full content of the page.
- **For Files**: Read the file content directly.
- **Analysis**: Identify the core capability, its name (kebab-case), and its primary use case.
- **Filter**: Apply information filtering principles to extract only high-value, high-density content.

### 2. Context Comparison (Deduplication)
- List the existing skills in your skills directory (typically, `.opencode/skills/`, `.claude/skills/`, or `~/.claude/skills/`).
- Compare the new capability against existing ones:
    - **Orthogonal**: If the functionality is unrelated to any existing skill, proceed to **Create**.
    - **Overlapping**: If a skill already exists for this domain (e.g., you found "Advanced JQ" and `jq-mastery` already exists), proceed to **Update**.

### 3. Creating a New Skill
- Create a new directory matching the skill name.
- Generate a `SKILL.md` with:
    - **YAML Frontmatter**: `name` and a concise `description` (max 1024 chars) focused on the core capability.
    - **Body**: Highly condensed instructions, "When to use" section, and essential examples only.
- **Content Density**: Ensure every sentence provides actionable value. Remove any explanatory fluff or verbose descriptions.
- **Proactive Script Creation**: Automatically create supporting scripts and references when they enhance usability:
    - **Scripts**: Create executable scripts for complex multi-step workflows, boilerplate generators, or automation tasks
    - **Assets**: Store configuration files, templates, reference data, or lookup tables
    - **Documentation**: Add companion READMEs for complex integrations or setup instructions
- Follow the [Agent Skill Specification Standard](https://agentskills.io/specification) for consistent formatting and structure.

### 4. Updating/Merging a Skill
- Read the existing `SKILL.md`.
- Append the new instructions or examples to the Markdown body.
- Update the `description` in the frontmatter if the skill's scope has expanded significantly.
- Ensure no duplicate frontmatter sections are created.

## Quality Standards

### Information Density Requirements
- **Maximum Value per Word**: Every sentence must contain actionable insight or essential pattern
- **No Redundancy**: Eliminate repetitive explanations or overlapping examples
- **Conciseness**: Aim for 50-70% reduction from source material while preserving 100% of essential capability
- **Action-Oriented**: Focus on "how to execute" rather than "what it is"

### Standards Adherence
- **Naming**: Use `lowercase-hyphenated-names`. Max 64 characters.
- **Description**: Ensure the description includes keywords that will help your discovery mechanism trigger the skill in the future.
- **Paths**: Use relative paths for any referenced scripts (e.g., `scripts/my-script.py`).
- **Script Integration**: Reference scripts from SKILL.md with clear usage instructions and prerequisites.

## Proactive Best Practices

### Script Discovery
- Always scan source documentation for repetitive command sequences
- Look for configuration patterns that can be templated
- Identify multi-step setup procedures that benefit from automation
- Extract common workflows into reusable scripts

### User Experience Focus
- Scripts should reduce cognitive load, not increase complexity
- Provide one-command solutions for common tasks
- Include clear error messages and next steps
- Design scripts to be discoverable and self-documenting

### Maintenance Considerations
- Keep scripts focused on single responsibilities
- Version control scripts alongside skill updates
- Document dependencies and prerequisites clearly
- Design scripts to be robust across different environments

## Example Transformations

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
