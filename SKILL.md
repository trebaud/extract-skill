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

### 4. Updating/Merging a Skill
- Read the existing `SKILL.md`.
- Append the new instructions or examples to the Markdown body.
- Update the `description` in the frontmatter if the skill's scope has expanded significantly.
- Ensure no duplicate frontmatter sections are created.

## Standards Adherence
- **Naming**: Use `lowercase-hyphenated-names`. Max 64 characters.
- **Description**: Ensure the description includes keywords that will help your discovery mechanism trigger the skill in the future.
- **Paths**: Use relative paths for any referenced scripts (e.g., `scripts/my-script.py`).

## Example Transformation
**Source Document**: "A guide to using the `sips` command on macOS to resize images."
**Action**: Create `image-resizing-mac`.
**Resulting Description**: "Resizes and converts image files using the macOS `sips` utility. Use when the user needs to batch process images or change formats via terminal."
