# Issue Fix Instructions

**You are operating in a GitHub Actions runner.**

The GitHub CLI is available as `gh` and authenticated via `GH_TOKEN`. Git is available and configured. You have write access to repository contents and can create branches, commits, pull requests, and comment on issues.

## Your Role
You are fixing issues in the PydanticAI Research Agent. Follow AGENTS.md (in the project root) for PydanticAI development principles and standards.

## Architecture Context
This is a Python-based AI agent system built with PydanticAI:
- **Agents**: Research agent (Brave Search) and Email agent (Gmail OAuth2)
- **Config**: Environment-based settings (python-dotenv), LLM model providers
- **Models**: Pydantic v2 models for email, research, and agent data
- **Tools**: Brave Search API integration, Gmail OAuth2 and draft creation
- **Testing**: TestModel and FunctionModel for agent validation
- **CLI**: Streaming interface using Rich library and PydanticAI's `.iter()` method

## Fix Workflow - FAST AND MINIMAL

### 1. GET ISSUE CONTEXT
**Use GitHub CLI to understand the issue:**
```bash
gh issue view <issue-number>
```
Read the issue description, comments, and any error messages or stack traces provided.

### 2. ROOT CAUSE ANALYSIS (RCA)
- **Identify**: Use ripgrep to search for error messages, function names, patterns
- **Trace**: Follow the execution path using git blame and code navigation
- **Root Cause**: What is the ACTUAL cause vs symptoms?
   - Is it a typo/syntax error?
   - Is it a logic error?
   - Is it a missing dependency?
   - Is it a type mismatch?
   - Is it an async/timing issue?
   - Is it a state management issue?

### 3. MINIMAL FIX STRATEGY
- **Scope**: Fix ONLY the root cause, nothing else
- **Pattern Match**: Look for similar code in the codebase - follow existing patterns
- **Side Effects**: Will this break anything else? Check usages with ripgrep
- **Alternative**: If fix seems too invasive, document alternative approaches

### 4. IMPLEMENTATION & PR CREATION
**CRITICAL: Use GitHub CLI (`gh`) for ALL GitHub operations**

1. **Make the fix**: Edit only the files needed to fix the root cause
2. **Create branch and commit**:
   ```bash
   git checkout -b fix/issue-{number}-{AI_ASSISTANT}
   git add <changed-files>
   git commit -m "fix: <brief description>"
   git push -u origin fix/issue-{number}-{AI_ASSISTANT}
   ```
3. **Create pull request** using `gh pr create`:
   ```bash
   gh pr create --title "Fix: <title>" --body "<description>"
   ```
   This will return the PR number - capture it for the next step.

**Branch naming**: `fix/issue-{number}-{AI_ASSISTANT}` or `fix/pr-{number}-{description}-{AI_ASSISTANT}` or `fix/{brief-description}-{AI_ASSISTANT}`

**PR Description Template**:
```
## Problem
[What was broken and why]

## Solution
[What you changed to fix it]

## Files Changed
- `path/to/file.py` - [what changed]

---
Fixed by {AI_ASSISTANT}
```

### 5. POST UPDATES TO ISSUE
**Use GitHub CLI to comment on the issue**:
```bash
gh issue comment <issue-number> --body "✅ Created PR #<pr-number> to fix this issue"
```

## Decision Points
- **Don't fix if**: Needs product decision, requires major refactoring, or changes core architecture
- **Document blockers**: If something prevents a complete fix, explain in PR and issue comment
- **Keep it simple**: No tests required - just fix and PR

## Remember
- **Always use `gh` CLI** for branches, commits, pushes, and PRs
- The person triggering this workflow wants a FAST fix - deliver one or explain why you can't
- Follow AGENTS.md for PydanticAI development principles and agent patterns
- Prefer ripgrep over grep for searching
- Keep changes minimal - resist urge to refactor
- Skip testing - just make the fix and create the PR
