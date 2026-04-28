---
name: alignment-analyzer
description: Analyzes scope-aware JIRA alignment. Evaluates Sub-tasks against their own description, not parent. Uses semantic matching for requirements. NON-BLOCKING.
---

# alignment-analyzer skill

**Single Responsibility:** Evaluate scope-aware JIRA alignment. NON-BLOCKING.

## Input

```json
{
  "diff_content": "...",
  "diff_stats": {"files_changed": 12, "additions": 450},
  "jira_context": {
    "issue_key": "ABC-123",
    "issue_data": {"type": "Sub-task", "summary": "...", "description": "..."},
    "parent_data": {"issue_key": "ABC-100", "type": "Story", "description": "..."}
  }
}
```

## What you do

### 1. If no JIRA context

Return `alignment: "UNKNOWN_SCOPE"`.

### 2. Determine scope source

Scope is always the issue's own description regardless of type (`scope_source: "issue_description"`).

### 3. Extract requirements

Parse scope description → identify actionable items.
Example: "Add JWT" → `["token generation", "validation"]`

### 4. Extract implementations

Parse diff → what was actually coded.
Example: `+generateToken()` → `"token generation"`

### 5. Semantic matching

Match requirements to implementations using synonyms:
- `"authentication"` = `["auth", "login", "OAuth"]`
- `"database"` = `["DB", "persistence", "storage"]`
- `"validation"` = `["validate", "check", "verify", "guard"]`

NOT keyword-only — use meaning.

### 6. Classify

- `matched == requirements` → `FULLY_ALIGNED`
- `0 < matched < requirements` → `PARTIALLY_ALIGNED`
- `matched == 0` → `NOT_ALIGNED`
- `matched == requirements AND extra_implementations > requirements_count` → `OVER_IMPLEMENTATION` (treat as `PARTIALLY_ALIGNED` in verdict)

## Output (JSON only)

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

**CRITICAL:** Sub-tasks evaluated against THEIR OWN description, NOT parent. Return ONLY valid JSON.
