# pr-preflight

Parses PR URLs into structured data and validates environment readiness before the PR review pipeline runs. Confirms git availability, MCP tool presence, and git identity. BLOCKING — pipeline must not continue on failure.

## Features

- Supports GitHub, GitLab (including sub-groups), and Bitbucket PR URLs
- Deterministic MCP availability check via tool name lookup
- Typed `BLOCK` / `FAILURE` / `SUCCESS` status for orchestrator routing
- Catastrophic failure contract — always emits valid JSON

## Installation

```bash
npx skills add https://github.com/cess15/skills --skill pr-preflight
```

## Input

PR URL string. Example:

```
https://bitbucket.org/workspace/repo/pull-requests/123
```

## Output

```json
{
  "agent_name": "pr-preflight",
  "version": "1.0",
  "status": "SUCCESS",
  "parsed_data": {
    "domain": "bitbucket.org",
    "provider": "bitbucket",
    "workspace": "acme",
    "repo_slug": "web-app",
    "pr_number": 123,
    "mcp_server": "mcp-server-atlassian-bitbucket"
  },
  "environment": {
    "git_available": true,
    "git_version": "2.40.0",
    "mcp_available": true,
    "git_identity": {
      "name": "John Doe",
      "email": "john@example.com"
    }
  },
  "errors": []
}
```

## Status values

| Value | Meaning |
|---|---|
| `SUCCESS` | All checks pass — pipeline may proceed |
| `BLOCK` | Hard failure — pipeline must stop (invalid URL, git missing, MCP not configured) |
| `FAILURE` | Partial failure — git present but identity missing or ambiguous |

## Supported URL patterns

| Provider | Pattern |
|---|---|
| GitHub | `github.com/<org>/<repo>/pull/<id>` |
| GitLab | `gitlab.com/<group>[/<subgroup>...]/<repo>/-/merge_requests/<id>` |
| Bitbucket | `bitbucket.org/<workspace>/<repo_slug>/pull-requests/<id>` |

## Pipeline position

Runs as the first BLOCKING step in the PR review pipeline, before any diff fetching or analysis:

```
PR URL
  └─► pr-preflight         — parse URL + validate env  [BLOCKING]
  └─► pr-fetcher           — fetch diff + metadata
  └─► jira-context         — fetch JIRA ticket data
  └─► security-compliance-review
  └─► alignment-analyzer
        └─► verdict: APPROVE / REQUEST_CHANGES
```
