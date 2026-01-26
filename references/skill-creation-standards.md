# Skill Specification & Standards

This document defines the Agent Skills specification format, structure requirements, and technical standards for creating compliant skills.

## Skill Specification

### Directory Structure
```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter
│   └── Markdown content body
└── bundled-resources/ (optional)
    ├── scripts/          # Executable code
    ├── references/       # Documentation 
    └── assets/           # Output files
```

### Naming Requirements
- **Skill Name**: `lowercase-hyphenated-format`, maximum 64 characters
- **Directory Match**: Directory name must exactly equal frontmatter `name` field
- **Description**: Maximum 1024 characters, include discovery keywords

### Progressive Disclosure System
Three-tier context loading:

1. **Metadata Layer** (always active): Name + description (~100 words)
2. **Core Layer** (when triggered): SKILL.md body (<500 lines total)
3. **Resource Layer** (as needed): Scripts, references, assets

### YAML Frontmatter Specification
Required frontmatter fields:
```yaml
---
name: skill-name              # Required: lowercase-hyphenated, max 64 chars
description: Skill purpose    # Required: max 1024 chars, discovery keywords
license: Apache-2.0           # Required: license identifier
metadata:
  version: 0.0.0            # Required: semantic version
  author: username           # Required: author identifier
---
```

### Content Requirements
- **SKILL.md Total Lines**: Maximum 500 lines (including frontmatter)
- **Body Content**: Under 5,000 words for efficient context loading
- **File References**: Use relative paths only (e.g., `scripts/tool.py`)
- **No Duplicate Sections**: Frontmatter fields must not be repeated in body

## Bundled Resources Specification

### Scripts Directory (scripts/)
**Purpose**: Executable code files
**File Types**: Python, Shell, JavaScript, etc.
**Requirements**: 
- Must be executable (appropriate file permissions)
- Self-contained with clear error handling
- Single responsibility per script
- Use relative imports within skill

**Use Cases**: 
- Repetitive code patterns
- Complex multi-step workflows
- Fragile operations requiring consistency
- Boilerplate generation

### References Directory (references/)
**Purpose**: Documentation for AI agent reference
**File Types**: Markdown, text files, schemas
**Requirements**:
- Must be referenced explicitly from SKILL.md
- One level deep from SKILL.md only
- No duplication with SKILL.md content

**Use Cases**:
- API documentation
- Database schemas
- Domain-specific knowledge
- Configuration guides

### Assets Directory (assets/)
**Purpose**: Files used in generated output
**File Types**: Templates, images, configuration files
**Requirements**:
- Binary or text files for output generation
- Not referenced by AI agent during execution

**Use Cases**:
- Template files
- Logos/images
- Configuration templates
- Boilerplate code

### File Organization Rules
- **Maximum Depth**: All bundled resources must be one level deep from SKILL.md
- **No Circular References**: Reference files should not reference each other
- **Explicit Navigation**: SKILL.md must clearly indicate when to read reference files
- **Unique Naming**: No filename conflicts across directories

## Content Organization Standards

### Progressive Disclosure Patterns

**Pattern 1: Navigation Guide**
```markdown
# API Integration

## Quick Start
Basic authentication flow:
[code example]

## Advanced Features
- **OAuth flows**: See [OAUTH.md](OAUTH.md) for complete implementation
- **Rate limiting**: See [RATE_LIMITING.md](RATE_LIMITING.md) for strategies
```

**Pattern 2: Domain Organization**
```
skill-name/
├── SKILL.md (overview and navigation guide)
└── references/
    ├── finance.md
    ├── sales.md  
    └── product.md
```

**Pattern 3: Conditional Details**
```markdown
# Data Processing

## Basic Operations
Simple filtering and transformation:
[code example]

**For advanced analytics**: See [ANALYTICS.md](ANALYTICS.md)
**For real-time processing**: See [STREAMING.md](STREAMING.md)
```

### Degrees of Freedom Classification

**High Freedom** (Text instructions):
- Multiple valid approaches
- Context-dependent decisions
- User-driven outcomes

**Medium Freedom** (Parameterized code):
- Preferred pattern exists
- Configurable behavior
- Structured flexibility

**Low Freedom** (Specific scripts):
- Error-prone operations
- Consistency requirements
- Deterministic execution needed

## Prohibited Content Standards

### Forbidden Files
These files must NOT be included in skills:
- README.md
- INSTALLATION_GUIDE.md  
- QUICK_REFERENCE.md
- CHANGELOG.md
- LICENSE.md (use frontmatter instead)
- Any duplicate documentation files

### Content Restrictions
Content that should be excluded from skills:
- Marketing language and promotional material
- Basic programming explanations
- Historical context or background information
- Repetitive examples without new patterns
- Non-essential descriptive content

### Quality Requirements
All content must be:
- Action-oriented and focused on execution
- High-density with actionable information
- Essential to the core capability
- Non-redundant across files

## Validation Requirements

### Structural Validation Rules
- **Frontmatter Completeness**: All required fields must be present and valid
- **Name-Directory Match**: Directory name must exactly equal frontmatter `name`
- **File Integrity**: All referenced files must exist and be accessible
- **Permission Requirements**: Scripts in `scripts/` must have execute permissions
- **Path Format**: All internal paths must use relative format

### Content Validation Standards
- **Line Count Limit**: SKILL.md must not exceed 500 lines total
- **Word Count Target**: Body content under 5,000 words for efficiency
- **Description Length**: Maximum 1,024 characters with discovery keywords
- **Actionable Content**: Focus on "Use when", "Apply", "Create", "Implement" patterns
- **No Marketing Language**: Exclude promotional words like "revolutionary", "cutting-edge"

### Format Compliance Check
- **Naming Conventions**: lowercase-hyphenated format, max 64 characters
- **YAML Syntax**: Valid frontmatter with required fields
- **Markdown Format**: Proper markdown structure throughout
- **File Organization**: One-level deep reference structure maintained

## Validation Commands

### Required Validation
All skills must pass validation before completion:
```bash
# Primary validation method
scripts/validate-skill ./skill-name

# Alternative validation method  
skills-ref validate ./skill-name
```

### Validation Response Types
- **PASS**: Skill complies with all standards and is ready for use
- **WARNINGS**: Review warnings but skill remains usable
- **ERRORS**: Critical compliance failures must be fixed

### Common Validation Fixes
- **Name Mismatch**: Ensure directory name matches frontmatter `name` field
- **Missing Frontmatter**: Add complete YAML frontmatter with required fields
- **Broken References**: Create referenced files or remove invalid links
- **Permission Issues**: Set execute permissions on scripts using `chmod +x`
- **Format Violations**: Fix naming conventions, file structure, or content organization



## Development Best Practices

### Script Development Standards
- **Single Responsibility**: Each script should handle one specific task
- **Error Handling**: Include clear error messages and next steps
- **Self-Documenting**: Use descriptive names and inline comments
- **Environment Robustness**: Design scripts to work across different systems
- **Dependency Documentation**: Clearly state prerequisites and requirements

### User Experience Guidelines
- **Cognitive Load Reduction**: Scripts should simplify, not complicate workflows
- **One-Command Solutions**: Provide single-command execution for common tasks
- **Discoverability**: Make scripts easily discoverable through naming and documentation
- **Clear Feedback**: Provide informative output and progress indicators

### Maintenance Principles
- **Version Alignment**: Keep scripts versioned with skill updates
- **Modular Design**: Design scripts to be maintainable and extensible
- **Testing Approach**: Include error testing scenarios
- **Documentation Sync**: Keep script documentation aligned with functionality

## Specification Compliance

### Skill Format Summary
This specification defines the complete requirements for Agent Skills:

1. **Directory Structure**: Required `SKILL.md` + optional bundled resources
2. **Naming Convention**: `lowercase-hyphenated`, max 64 chars, directory match required
3. **Content Limits**: SKILL.md max 500 lines, body under 5,000 words
4. **Progressive Disclosure**: Three-tier loading system for efficiency
5. **Resource Organization**: One-level deep, purpose-specific directories

### Compliance Checklist
- [ ] Frontmatter with all required fields (name, description, license, metadata)
- [ ] Directory name matches frontmatter name field
- [ ] Description under 1,024 characters with discovery keywords
- [ ] SKILL.md under 500 lines total
- [ ] All referenced files exist and are accessible
- [ ] Scripts in `scripts/` directory have execute permissions
- [ ] No prohibited files (README.md, CHANGELOG.md, etc.)
- [ ] Content passes validation without critical errors

This specification serves as the authoritative reference for Agent Skills format and structural requirements.