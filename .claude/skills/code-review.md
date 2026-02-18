# PR Review Agent Skill

**Skill Name**: `pr-review`
**Aliases**: `review-pr`, `analyze-pr`
**Version**: 1.0.0

## Description

Conducts comprehensive PR reviews in your personal style, analyzing code consistency, security patterns, and providing constructive feedback with enthusiasm and technical insights that match your established review approach.

## Usage

```
/pr-review <github-pr-url> [--mode=quick|thorough|security-focused] [--post-review]
```

### Examples

```bash
# Quick review (default - 5-10 minutes)
/pr-review https://github.com/monday-com/automations/pull/123

# Thorough review (20-30 minutes)
/pr-review https://github.com/monday-com/automations/pull/123 --mode=thorough

# Security-focused review
/pr-review https://github.com/monday-com/automations/pull/123 --mode=security-focused

# Review and post to GitHub (requires auth)
/pr-review https://github.com/monday-com/automations/pull/123 --post-review
```

## Personal Review Style

This skill replicates your specific review characteristics:

### Communication Approach
- **Positive reinforcement**: "Nice consistency with existing pattern!"
- **Enthusiasm with emojis**: "🥳" for good changes
- **Constructive suggestions**: "maybe we can..." approach
- **Brief, actionable feedback**: Clear next steps without overwhelming detail

### Technical Focus Areas
- **Code consistency and architectural patterns** (your top priority)
- **Flexibility and extensibility**: "good flexibility for custom display names"
- **Security and authorization patterns**
- **Monday.com coding standards** from CLAUDE.md
- **TypeScript best practices** (interfaces over types, proper return types)

## Implementation

The skill uses Claude Code's built-in tools and doesn't require custom code in the repository.

### Tools Used
- `Bash` with `gh` CLI for GitHub API access
- `WebFetch` for PR content retrieval
- `Grep` and `Glob` for codebase context analysis
- Built-in code analysis capabilities

### Skill Logic

1. **Parse GitHub PR URL** to extract owner/repo/number
2. **Fetch PR data** using `gh pr view` and `gh pr diff`
3. **Analyze changes** using Claude's code analysis with your style prompts
4. **Generate review** matching your communication patterns
5. **Format output** with appropriate emojis and structure
6. **Optionally post** review to GitHub if `--post-review` flag used

## Review Modes

### Quick Mode (Default)
- Focus on critical issues and patterns
- 5-10 minute analysis timeframe
- Highlights major architectural concerns
- Brief, actionable suggestions

### Thorough Mode
- Deep architectural analysis
- 20-30 minute analysis timeframe
- Comprehensive feedback on all aspects
- Detailed explanations and context

### Security-Focused Mode
- Emphasis on authorization patterns
- Vulnerability detection
- Secrets and sensitive data exposure
- Security best practices validation

## Output Format

```markdown
✅ **PR Review Complete** - APPROVED

## Summary
Nice consistency with existing patterns! 🥳 This implementation follows Monday.com coding standards well and shows good architectural thinking.

## Code Review Comments (3)

1. **src/services/new-service.ts:15** ℹ️
   Consider using interface instead of type for better consistency with project patterns.

2. **src/controllers/new-controller.js:42** ⚠️
   Maybe we can add proper error handling here to match other controller patterns.

3. **src/constants/monday-constants.ts:3** 💬
   Good choice of ID -12 following the established negative ID pattern for special users.

## Security Concerns
🔒 No security issues detected

## Architectural Notes
🏗️ Consider centralizing special user handling logic to reduce duplication
🏗️ The pattern follows established Monday.com conventions well
```

## Configuration

The skill works out of the box with Claude Code's GitHub integration. No additional configuration required in the codebase.

### Optional Environment Variables
- `GITHUB_TOKEN`: For GitHub API access (usually auto-configured by Claude Code)
- `GH_TOKEN`: Alternative GitHub token variable

## Integration with Monday.com Workflow

The skill understands Monday.com specific patterns:
- Trident framework conventions
- Configuration service patterns
- Monday.com coding standards from CLAUDE.md
- TypeScript migration best practices
- Service architecture patterns

## Error Handling

- Validates GitHub PR URLs
- Handles private repository access
- Graceful fallback for API rate limits
- Clear error messages for auth issues