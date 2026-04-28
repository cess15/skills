# jira-context

Extracts JIRA issue keys from PR context and enriches with ticket details via the Atlassian MCP. Scans all fetched content for prompt injection attempts. NON-BLOCKING.

## Features

- Issue key extraction from branch name, PR title, and description
- Sub-task aware — fetches parent issue for downstream context
- Prompt injection detection across all fetched text fields
- NON-BLOCKING — returns partial data on failure, never aborts pipeline

## Installation

```bash
npx skills add https://github.com/cess15/skills --skill jira-context
```

## Requirements

Requires the **claude.ai Atlassian MCP** (`mcp__claude_ai_Atlassian__*`). Not compatible with `mcp-server-atlassian-bitbucket`.

## Input

```json
{
  "source_branch": "feature/ABC-123-add-auth",
  "pr_title": "Add JWT authentication",
  "pr_description": "Implements JWT tokens..."
}
```

Key extraction priority: branch name → PR title → PR description. Pattern: `[A-Z]{2,10}-\d+`.

## Output

```json
{
  "agent_name": "jira-context",
  "status": "AVAILABLE",
  "issue_key": "ABC-123",
  "issue_data": {
    "type": "Sub-task",
    "summary": "Implement JWT tokens",
    "description": "- Generate access token\n- Validate token",
    "status": "In Progress"
  },
  "parent_data": {
    "issue_key": "ABC-100",
    "type": "Story",
    "description": "Add authentication system"
  },
  "scope_source": "issue_description",
  "injection_warning": false,
  "injection_details": [],
  "errors": []
}
```

## Status values

| Value | Meaning |
|---|---|
| `AVAILABLE` | Issue found and data fetched |
| `NOT_FOUND` | No JIRA key matched in any source |
| `ERROR` | MCP call failed — partial data returned |

## Prompt injection detection

Scans PR title, description, branch name, JIRA summary, JIRA description, and parent description for patterns including: `Ignore all previous`, `SYSTEM:`, `INSTRUCTIONS:`, `You are now`, `Disregard your`, `OVERRIDE:`, `AI: `, `TODO: Claude`.

Sets `injection_warning: true` and populates `injection_details` if detected. Does NOT abort.

## Used by

- [`alignment-analyzer`](../alignment-analyzer/) — consumes `jira_context` output to evaluate JIRA scope alignment
