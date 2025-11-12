# Working with Claude AI Agents

This guide explains how to effectively work with the multi-agent system powered by Claude in Cursor.

## Quick Start

### Activating an Agent

To activate a specific agent, use the `@` mention syntax in your chat:

```
@business-analyst Analyze this new feature request for user authentication
```

```
@software-engineer Implement LIN-456 for the checkout flow
```

```
@senior-engineer Review the PR for LIN-789 and provide architectural feedback
```

## Agent Personalities

### 🎯 Business Analyst Agent

**When to use:**
- You have a business opportunity or feature idea
- You need to create Linear issues
- You want to refine requirements
- You need to break down complex features

**What to provide:**
- Business context and goals
- Target users or personas
- Expected outcomes
- Any constraints or dependencies

**Example conversation:**
```
@business-analyst We need to add a notification system for users.

Users are complaining they miss important updates. We want to 
increase user engagement by 30% over the next quarter. The system 
should support email and in-app notifications initially.
```

**What you'll get:**
- A parent Linear issue with business value
- Multiple sub-issues for atomic implementation
- Clear acceptance criteria
- Implementation hypotheses
- Edge cases and considerations

---

### 💻 Software Engineer Agent

**When to use:**
- You have a Linear issue to implement
- You need to write code following standards
- You want comprehensive tests
- You're ready to create a PR

**What to provide:**
- Linear issue ID
- Any clarifications on acceptance criteria
- Technical constraints or preferences
- Related code or context

**Example conversation:**
```
@software-engineer Implement LIN-456

Here's the Linear issue for adding email notifications.
The main branch has been updated with new auth changes,
so make sure to rebase before starting.
```

**What you'll get:**
- A feature branch with proper naming
- Production-ready code
- Unit, integration, and e2e tests
- Security scan results
- A pull request ready for review

---

### 🏗️ Senior Software Engineer Agent

**When to use:**
- You need architectural guidance
- You want to review code or PRs
- You need to prioritize issues
- You want to establish coding standards
- You need to make technical decisions

**What to provide:**
- Context about the problem
- Options you're considering
- Trade-offs you've identified
- Code to review (if applicable)

**Example conversation:**
```
@senior-engineer I need guidance on state management.

We're building a real-time collaboration feature. Should we use:
1. Nanostores for simple state
2. Redux for complex state management
3. Server-sent events with minimal client state

Performance and maintainability are priorities.
```

**What you'll get:**
- Architectural recommendations
- Pros/cons of different approaches
- Best practices for your tech stack
- Code review with actionable feedback
- Updated coding standards if needed

## Common Workflows

### 1. New Feature from Scratch

```
You: We need to add two-factor authentication to improve security.

@business-analyst Please analyze this feature request and create 
Linear issues.

[BA creates issues]

You: Thanks! @senior-engineer Please review these issues and 
prioritize them.

[Senior reviews and prioritizes]

You: @software-engineer Please implement LIN-123 (the first issue).

[Engineer implements]

You: @senior-engineer Please review PR #45.

[Senior reviews and approves]
```

### 2. Implementing an Existing Issue

```
You: @software-engineer Implement LIN-789

[Engineer asks clarifying questions if needed]

You: [Provides clarifications]

[Engineer implements, tests, and creates PR]

You: @senior-engineer Review PR #46

[Senior provides feedback]

You: @software-engineer Address the review comments in PR #46

[Engineer makes changes and updates PR]
```

### 3. Getting Architectural Guidance

```
You: @senior-engineer We're adding a caching layer. Should we use 
Redis or in-memory caching? We have 100k daily users.

[Senior provides analysis and recommendation]

You: Makes sense! @business-analyst Update LIN-345 with the 
Redis approach.

[BA updates issue]

You: @software-engineer Implement the caching using the approach 
defined in LIN-345.
```

### 4. Troubleshooting

```
You: @software-engineer The tests are failing in CI for PR #47.

[Engineer investigates and fixes]

You: @senior-engineer The fix required changing the architecture 
slightly. Can you review?

[Senior reviews and approves or suggests alternatives]
```

## Best Practices

### Clear Context

Always provide sufficient context:

```
❌ Bad:
@software-engineer Fix the bug

✅ Good:
@software-engineer Fix the authentication bug in LIN-456

Users can't log in with email addresses containing '+' character.
The issue is in the validation regex in src/lib/utils/auth.ts.
Tests should cover edge cases like: user+test@example.com
```

### One Agent at a Time

Don't mix agent responsibilities:

```
❌ Bad:
@business-analyst @software-engineer Create issue and implement it

✅ Good:
@business-analyst Create a Linear issue for user profile editing

[Wait for issue creation]

@software-engineer Implement LIN-890
```

### Reference Previous Work

Link to related issues, PRs, or code:

```
✅ Good:
@senior-engineer Review PR #48

This is related to LIN-123 and PR #45 we did last week.
The new approach uses the same pattern but for a different entity.
```

### Provide Files and Code

Include relevant code when asking for help:

```
✅ Good:
@senior-engineer I'm getting a TypeScript error in this file:

[Share file or paste code]

The error is on line 45: Type 'string' is not assignable to type 'User'.
```

## Agent Limitations

### What Agents CAN Do

- ✅ Read and analyze code in the repository
- ✅ Create, modify, and delete files
- ✅ Run tests and linters
- ✅ Search codebase
- ✅ Create branches and commits
- ✅ Run Snyk security scans
- ✅ Access Linear (if configured)
- ✅ Review pull requests

### What Agents CANNOT Do

- ❌ Push directly to protected branches
- ❌ Merge pull requests (requires human approval)
- ❌ Access external systems (databases, APIs) directly
- ❌ Run long-running services
- ❌ Make irreversible infrastructure changes

## Configuration

### Per-Repository Setup

Each repository should have:

1. **Agent rules** in `.cursor/rules/`:
   - `business-analyst.mdc`
   - `software-engineer.mdc`
   - `senior-engineer.mdc`
   - Technology-specific rules (e.g., `react.mdc`, `python.mdc`)

2. **Documentation**:
   - `AGENTS.md` - System overview
   - `CLAUDE.md` - This file
   - Repository-specific README

3. **Linear integration**:
   - Configure Linear workspace
   - Set up project structure
   - Create issue templates

### Cursor Settings

Enable agent rules in `.cursor/rules/`:

```json
{
  "cursor.rules": [
    ".cursor/rules/business-analyst.mdc",
    ".cursor/rules/software-engineer.mdc",
    ".cursor/rules/senior-engineer.mdc"
  ]
}
```

## Tips and Tricks

### 1. Iterative Refinement

Don't expect perfection on first try:

```
You: @software-engineer Implement LIN-123

[Reviews implementation]

You: The implementation looks good, but can you add more 
edge case tests?

[Engineer adds tests]

You: Perfect! @senior-engineer Please review.
```

### 2. Explicit Instructions

Be specific about what you want:

```
✅ Good:
@business-analyst Create a Linear issue for dark mode

Include:
- User benefit (accessibility, preference)
- Technical approach (CSS variables, localStorage)
- Acceptance criteria (toggle, persistence, system preference)
- Break into sub-issues: UI, persistence, system detection
```

### 3. Security First

Always run security scans for new code:

```
@software-engineer Implement LIN-456 and run Snyk scan

If security issues are found, fix them before creating the PR.
```

### 4. Knowledge Sharing

Use agents to document decisions:

```
@senior-engineer Document the decision to use Redis

Create an ADR (Architecture Decision Record) explaining:
- Context: Need for caching
- Decision: Redis over in-memory
- Consequences: Better scalability, added infrastructure
```

## Troubleshooting

### Agent Not Responding as Expected

1. Check that you're using the correct agent mention
2. Ensure the agent rules are loaded (check Cursor settings)
3. Provide more context in your request
4. Try rephrasing your request

### Agent Creating Wrong Code

1. Check if coding standards are defined in repository
2. Provide examples of desired code style
3. Reference existing code to follow
4. Ask Senior Engineer to update standards

### Tests Failing

1. Ask Software Engineer to run tests locally
2. Check CI logs for specific errors
3. Ensure dependencies are up to date
4. Ask Senior Engineer for architectural review

## Getting Help

If you're stuck:

1. **Check documentation**: `AGENTS.md` and this file
2. **Ask Senior Engineer**: `@senior-engineer I need help with [problem]`
3. **Provide context**: Share error messages, code, and what you've tried
4. **Be specific**: Vague questions get vague answers

## Examples by Use Case

### Code Review

```
@senior-engineer Review this authentication implementation in 
src/lib/auth.ts

Focus on:
1. Security best practices
2. Error handling
3. TypeScript type safety
4. Test coverage
```

### Bug Fix

```
@software-engineer Fix bug LIN-567

Users report that dates are displayed incorrectly in UTC instead 
of local timezone. The issue appears to be in the formatDate 
utility. Please add tests covering different timezones.
```

### Refactoring

```
@senior-engineer Should we refactor the user service?

It's grown to 500+ lines and handles authentication, profile 
management, and notifications. I'm thinking of splitting it into:
1. AuthService
2. ProfileService  
3. NotificationService

What do you think?
```

### Performance Optimization

```
@software-engineer The dashboard page is slow (3s load time)

Profile the page, identify bottlenecks, and optimize:
1. Check bundle size
2. Implement lazy loading for heavy components
3. Add caching where appropriate
4. Run Lighthouse audit before/after
```

## Advanced Usage

### Chaining Agents

For complex workflows:

```
@business-analyst Create issue for API rate limiting

[Creates issue LIN-999]

@senior-engineer Review LIN-999 and provide technical approach

[Provides guidance]

@business-analyst Update LIN-999 with technical details

[Updates issue]

@software-engineer Implement LIN-999

[Implements]

@senior-engineer Review PR #99

[Reviews and approves]
```

### Custom Workflows

Define repository-specific workflows:

```
@senior-engineer Create a custom workflow for hotfixes

We need a fast-track process for production issues that:
1. Bypasses normal issue creation
2. Creates hotfix branch from main
3. Requires minimal tests
4. Gets expedited review
5. Documents post-mortem
```

## Conclusion

The multi-agent system is designed to streamline development while maintaining quality. Each agent has specific expertise—use them appropriately for best results.

Key takeaways:
- Use the right agent for the right task
- Provide clear context and requirements
- Follow the workflow: Requirements → Implementation → Review
- Iterate based on feedback
- Document important decisions

Happy coding! 🚀

