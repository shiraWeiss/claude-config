# Claude Skills

This directory contains custom skills for Claude Code. Skills are specialized prompts that can be invoked using the `/skill-name` syntax or through the Skill tool.

## Usage

Skills can be invoked in two ways:

1. **Slash command**: `/skill-name [args]`
2. **Skill tool**: Using the Skill tool with the skill name and optional arguments

## Creating Skills

Each skill is a folder containing a `SKILL.md` that starts with YAML frontmatter (`name`, `description`). To use a skill globally, symlink its folder into `~/.claude/skills/`. Each skill should contain:

- A clear description of what the skill does
- The prompts and instructions for Claude
- Any specific parameters or arguments the skill accepts

## Examples

- `/my-skill` - Run a custom skill
- `/my-skill arg1 arg2` - Run a skill with arguments

## File Structure

```
.claude/skills/
├── README.md          # This file
├── pr-review/SKILL.md
└── writing-code/SKILL.md
```