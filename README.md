# Extract Skill

A meta Agent Skill that enables automated capability growth by extracting specialized knowledge from documentation and transforming it into standardized Agent Skills.

## Overview

`extract-skill` follows the [Agent Skill Specification](https://agentskills.io/specification) to parse technical documents, README files, API guides, and tutorials into concise, reusable skills. It applies aggressive information filtering to extract only high-value, information dense content.

## When to Use

- Converting URLs with guides, tutorials, or API references into reusable skills
- Processing local files (PDF, Markdown, Text, JSON) describing specific workflows  
- Formalizing discovered patterns into permanent capabilities within chat sessions

## Key Features

- **Information Filtering**: Extracts only high-value, high-density information (50-70% reduction from source)
- **Deduplication**: Compares against existing skills to avoid overlap
- **Standardization**: Follows Agent Skill Specification for consistent formatting
- **Quality Control**: Ensures maximum actionable insight per word with no redundancy
- **Proactive Script Creation**: Automatically creates supporting scripts, templates, and assets to enhance skill usability
- **Automation-First**: Identifies workflows that benefit from automation and creates executable solutions

## Workflow

1. **Ingest**: Fetch content from URLs or read local files
2. **Filter**: Apply information filtering principles to extract essential patterns
3. **Compare**: Check against existing skills for deduplication
4. **Create/Update**: Generate new skills or merge with existing ones
5. **Automate**: Create supporting scripts, templates, and assets for enhanced usability

## Quality Standards

- `lowercase-hyphenated-names` (max 64 characters)
- Concise descriptions with discovery keywords
- Relative paths for referenced scripts
- Action-oriented instructions focused on execution
- Proactive creation of automation scripts and reference assets
- One-command solutions for complex workflows

Transforms verbose documentation into precise, executable capabilities with automated workflows while preserving 100% of essential techniques.
