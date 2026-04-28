# alignment-analyzer

Scope-aware JIRA alignment analysis for PR diffs. Evaluates how well a PR implements the requirements in its linked JIRA ticket using semantic matching — not keyword search.

## Features

- Sub-task scope uses the sub-task's own description, never the parent
- Semantic synonym matching (`"auth"` matches `"authentication"`, `"login"`, `"OAuth"`)
- Alignment classification with per-requirement confidence scoring
- NON-BLOCKING — returns `UNKNOWN_SCOPE` gracefully when no JIRA context is available

## Installation

```bash
npx skills add https://github.com/cess15/skills --skill alignment-analyzer
```

## Input

```json
{
  "diff_content": "...",
  "diff_stats": { "files_changed": 12, "additions": 450 },
  "jira_context": {
    "issue_key": "ABC-123",
    "issue_data": { "type": "Sub-task", "summary": "...", "description": "..." },
    "parent_data": { "issue_key": "ABC-100", "type": "Story", "description": "..." }
  }
}
```

`jira_context` is produced by the [`jira-context`](../jira-context/) skill. If absent, returns `alignment: "UNKNOWN_SCOPE"`.

## Output

```json
{
  "agent_name": "alignment-analyzer",
  "status": "SUCCESS",
  "alignment": "FULLY_ALIGNED",
  "analysis": {
    "scope_source": "issue_description",
    "requirements_count": 2,
    "implementations_count": 2,
    "matched_count": 2,
    "unmatched_requirements": [],
    "extra_implementations": []
  },
  "details": {
    "requirements": ["Generate access token", "Validate token"],
    "implementations": ["JWT generation logic", "Token validation middleware"],
    "matches": [
      {
        "requirement": "Generate access token",
        "implementation": "JWT generation logic",
        "confidence": 0.95
      }
    ]
  },
  "reasoning": "PR fully implements sub-task scope."
}
```

## Alignment values

| Value | Meaning |
|---|---|
| `FULLY_ALIGNED` | All requirements matched |
| `PARTIALLY_ALIGNED` | Some requirements matched |
| `NOT_ALIGNED` | No requirements matched |
| `OVER_IMPLEMENTATION` | All matched + extra implementations beyond scope (treated as `PARTIALLY_ALIGNED` in verdict) |
| `UNKNOWN_SCOPE` | No JIRA context available |

## Verdict mapping

| Alignment | Verdict |
|---|---|
| `FULLY_ALIGNED` | → APPROVE (if no security issues) |
| `PARTIALLY_ALIGNED` | → REQUEST_CHANGES (if missing critical elements) |
| `OVER_IMPLEMENTATION` | → REQUEST_CHANGES (treated as PARTIALLY_ALIGNED) |
| `NOT_ALIGNED` | → REQUEST_CHANGES |
| `UNKNOWN_SCOPE` | → no alignment block |
