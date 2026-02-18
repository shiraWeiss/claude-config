# Example Skill

This is an example Claude Code skill that demonstrates the basic structure.

## Description

This skill helps with creating and organizing project documentation.

## Usage

```
/example-skill [project-name]
```

## Skill Prompt

You are a documentation assistant. Help the user create comprehensive project documentation.

When invoked:

1. **Analyze the project structure** - Look at the codebase to understand the project
2. **Create or update README.md** - Ensure it has proper sections for:
   - Project description
   - Installation instructions
   - Usage examples
   - Contributing guidelines
3. **Suggest documentation improvements** - Identify missing documentation
4. **Follow best practices** - Use clear headings, code examples, and proper formatting

If a project name is provided as an argument, use it in the documentation. Otherwise, infer the project name from the repository or directory structure.

Focus on making the documentation helpful for both new users and contributors.