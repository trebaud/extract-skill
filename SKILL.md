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

**CRITICAL**: Extract only high-value, high-density information. The context window is a shared resource—every word must justify its token cost.

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

### Context Window Optimization

**Default assumption: AI agents are already very smart.** Only add context agents don't already have. Challenge each piece of information: "Does the agent really need this explanation?" and "Does this paragraph justify its token cost?"

Prefer concise examples over verbose explanations. The skill shares context with system prompts, conversation history, and other skills—efficiency is non-negotiable.

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
- **Filter**: Apply information filtering principles to extract only high-value, high-density content.

### Step 3: Context Comparison (Deduplication)

- List the existing skills in your skills directory (typically, `.opencode/skills/`, `.ai/skills/`, or `~/.ai/skills/`).
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

Follow the progressive disclosure patterns and bundled resources strategy outlined above.

### 3. Creating a New Skill

#### Skill Anatomy
```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (name, description)
│   └── Markdown instructions (loaded after triggering)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code for deterministic reliability
    ├── references/       - Documentation loaded as needed into context
    └── assets/           - Files used in output (templates, icons, etc.)
```

#### Progressive Disclosure Design
Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words, ideally <500 lines)
3. **Bundled resources** - As needed by the AI agent (unlimited, scripts execute without context loading)

**Keep SKILL.md body to essentials only.** Split content into separate files when approaching limits. Always reference these files from SKILL.md and clearly describe when to read them.

##### Progressive Disclosure Patterns

**Pattern 1: High-level guide with references**
```markdown
# API Integration

## Quick start
Basic authentication flow:
[code example]

## Advanced features
- **OAuth flows**: See [OAUTH.md](OAUTH.md) for complete guide
- **Rate limiting**: See [RATE_LIMITING.md](RATE_LIMITING.md) for strategies
- **Error handling**: See [ERRORS.md](ERRORS.md) for patterns
```

**Pattern 2: Domain-specific organization**
```
skill-name/
├── SKILL.md (overview and navigation)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    └── product.md (API usage, features)
```

**Pattern 3: Conditional details**
Show basic content, link to advanced:
```markdown
# Data Processing

## Basic operations
Simple filtering and transformation:
[code example]

**For advanced analytics**: See [ANALYTICS.md](ANALYTICS.md)
**For real-time processing**: See [STREAMING.md](STREAMING.md)
```

#### Content Creation Guidelines
- **YAML Frontmatter**: `name` and concise `description` (max 1024 chars) focused on when to use the skill
- **Body**: Highly condensed instructions and essential examples only
- **Content Density**: Every sentence must provide actionable value
- **No redundancy**: Information should live in either SKILL.md or reference files, never both
- **Proactive Script Creation**: Automatically create supporting scripts and references when they enhance usability

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

## Bundled Resources Strategy

### Scripts (`scripts/`)

Executable code for tasks requiring deterministic reliability or repeated execution.

**When to include scripts**:
- When the same code is being rewritten repeatedly
- When deterministic reliability is needed
- For complex multi-step workflows requiring repeated command sequences
- For code generation patterns that create boilerplate or templates
- For operations that are fragile and error-prone (low freedom)

**Examples**: `scripts/rotate_pdf.py`, `scripts/setup.sh`, `scripts/generate-config.py`

**Benefits**: Token efficient, deterministic, may be executed without loading into context

### References (`references/`)

Documentation and reference material loaded as needed into context to inform the AI agent's process.

**When to include references**:
- For documentation that the AI agent should reference while working
- Database schemas, API documentation, domain knowledge
- Company policies, detailed workflow guides
- When supporting multiple domains, frameworks, or variants

**Examples**: `references/finance.md`, `references/api_docs.md`, `references/schemas.md`

**Benefits**: Keeps SKILL.md lean, loaded only when needed
**Best practice**: If files are large (>10k words), include grep search patterns in SKILL.md
**Avoid duplication**: Information should live in either SKILL.md or references files, never both

### Assets (`assets/`)

Files not intended to be loaded into context, but used within the output the AI agent produces.

**When to include assets**:
- When the skill needs files that will be used in the final output
- Templates, images, icons, boilerplate code, fonts, sample documents
- Configuration files that get copied or modified

**Examples**: `assets/logo.png`, `assets/slides.pptx`, `assets/frontend-template/`

**Benefits**: Separates output resources from documentation, enables use without context loading

### What NOT to Include

Do NOT create extraneous documentation or auxiliary files:
- README.md
- INSTALLATION_GUIDE.md  
- QUICK_REFERENCE.md
- CHANGELOG.md
- etc.

The skill should only contain essential information that directly supports its functionality. Creating additional documentation files adds clutter and confusion.

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
