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

## Workflow

### 1. Ingest Source Content
- **For URLs**: Use `curl` or a browsing tool to fetch the full content of the page.
- **For Files**: Read the file content directly.
- **Analysis**: Identify the core capability, its name (kebab-case), and its primary use case.

### 2. Context Comparison (Deduplication)
- List the existing skills in your skills directory (typically, `.opencode/skills/`, `.claude/skills/`, or `~/.claude/skills/`).
- Compare the new capability against existing ones:
    - **Orthogonal**: If the functionality is unrelated to any existing skill, proceed to **Create**.
    - **Overlapping**: If a skill already exists for this domain (e.g., you found "Advanced JQ" and `jq-mastery` already exists), proceed to **Update**.

### 3. Creating a New Skill
- Create a new directory matching the skill name.
- Generate a `SKILL.md` with:
    - **YAML Frontmatter**: `name` and a concise `description` (max 1024 chars).
    - **Body**: Structured instructions, "When to use" section, and clear examples.
- If the document provides specific scripts or complex templates, save them in `scripts/` or `assets/` subdirectories.
- Follow the [Agent Skill Specification Standard](https://agentskills.io/specification) for consistent formatting and structure.

### 4. Updating/Merging a Skill
- Read the existing `SKILL.md`.
- Append the new instructions or examples to the Markdown body.
- Update the `description` in the frontmatter if the skill's scope has expanded significantly.
- Ensure no duplicate frontmatter sections are created.

## Standards Adherence
- **Naming**: Use `lowercase-hyphenated-names`. Max 64 characters.
- **Description**: Ensure the description includes keywords that will help your discovery mechanism trigger the skill in the future.
- **Paths**: Use relative paths for any referenced scripts (e.g., `scripts/my-script.py`).

## Example Transformations

<example>
**Source Document**: "My Claude conversation history JSON file containing patterns of how I solve problems."
**Action**: Create `problem-solving-patterns`.
**Resulting Description**: "Identifies and applies recurring problem-solving patterns from your conversation history. Use when you need to approach new challenges using your proven analytical methods."
</example>

<example>
**Source Document**: "Tech blog tutorial on implementing OAuth 2.0 with Node.js and Express."
**Action**: Create `oauth2-nodejs-implementation`.
**Resulting Description**: "Implements OAuth 2.0 authentication flows in Node.js applications using Express. Use when you need to add secure user authentication and authorization to web APIs."
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
</example>
