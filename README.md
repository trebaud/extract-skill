# Extract Skill

Transforms documentation/blog posts/tutorials into Agent Skills using epistemic distillation to extract only essential patterns. Follows the [Agent Skills specification](https://agentskills.io/specification).

## Use Cases
- Convert URLs/tutorials into reusable skills
- Process local files (PDF, MD, TXT, JSON) with workflows
- Formalize discovered patterns into capabilities

## Core Features
- Significant content reduction using epistemic distillation
- Progressive disclosure: metadata → SKILL.md → resources
- Deduplication against existing skills
- Degrees of freedom matching (high/medium/low)
- Agent Skills specification compliance

## Workflow
1. Ingest content from URLs or files
2. Apply distillation heuristics
3. Compare with existing skills
4. Generate skill with progressive disclosure
5. Validate with `scripts/validate-skill ./skill-name`

## Examples
Skills created using extract-skill can be found in the `examples/` directory.

## Quality Standards
- Names: `lowercase-hyphenated` (≤64 chars)
- Context-optimized descriptions
- Action-oriented instructions only
- No extraneous documentation
