# AI Agent Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Reusable AI agent skills for code review, security analysis, and JIRA alignment validation.

## Skill Catalog

| Skill | Purpose | Blocking |
|---|---|---|
| [security-compliance-review](skills/security-compliance-review/) | SAST + compliance analysis on PR diffs | NON-BLOCKING |
| [alignment-analyzer](skills/alignment-analyzer/) | JIRA scope alignment via semantic matching | NON-BLOCKING |
| [jira-context](skills/jira-context/) | JIRA ticket enrichment + prompt injection detection | NON-BLOCKING |

## Quick Install

```bash
# Install a specific skill
npx skills add https://github.com/cess15/skills --skill <skill-name>
```

## Pipeline

These skills are designed to run together in a PR review pipeline:

```
PR diff
  └─► jira-context          — fetch JIRA ticket data
  └─► security-compliance-review  — SAST + compliance
  └─► alignment-analyzer    — evaluate scope coverage
        └─► verdict: APPROVE / REQUEST_CHANGES
```

## Repository Structure

```
skills/
├── README.md
├── LICENSE
├── skills/
│   ├── security-compliance-review/
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── examples/
│   ├── alignment-analyzer/
│   │   ├── README.md
│   │   └── SKILL.md
│   └── jira-context/
│       ├── README.md
│       └── SKILL.md
├── spec/
│   └── agent-skills-spec.md
└── template/
    └── SKILL.md
```

## Contributing

1. Copy `template/SKILL.md`
2. Create your skill under `skills/your-skill-name/`
3. Add `README.md` and `SKILL.md`
4. Submit a PR

## License

MIT — see [LICENSE](LICENSE).
