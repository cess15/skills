# AI Agent Skills

> A collection of reusable AI agent skills for code review, security analysis, and compliance validation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Skills](https://img.shields.io/badge/Skills-AI%20Agent-blue)](https://skills.sh)

A curated collection of AI agent skills focused on security analysis, compliance validation, and software development best practices.

## 📦 Available Skills

### 🔒 security-compliance-review

AI-powered SAST and compliance analysis for Pull Request diffs. Detects vulnerabilities and regulatory compliance issues across any programming language.

**Features:**
- 🔍 Advanced SAST Analysis (OWASP Top 10)
- 📋 Compliance Checks (GDPR, HIPAA, SOC2, PCI-DSS)
- 🎯 Diff-Only Analysis
- 💥 Exploitability Assessment
- 🏷️ CWE Mapping
- 📊 Risk Scoring (0-10 scale)

**Installation:**
```bash
npx skills add https://github.com/cess15/skills --skill security-compliance-review
```

[📖 Full Documentation](skills/security-compliance-review/)

---

### 🎯 alignment-analyzer

Scope-aware JIRA alignment analysis for PR diffs. Evaluates Sub-tasks against their own description (not the parent), using semantic matching to classify implementation coverage.

**Features:**
- 🔍 Semantic requirement matching (not keyword-only)
- 🧩 Sub-task vs Story scope disambiguation
- 📊 Alignment classification: FULLY_ALIGNED / PARTIALLY_ALIGNED / NOT_ALIGNED / OVER_IMPLEMENTATION
- 🔗 Per-requirement confidence scoring
- 🚦 NON-BLOCKING — returns UNKNOWN_SCOPE gracefully when no JIRA context

**Installation:**
```bash
npx skills add https://github.com/cess15/skills --skill alignment-analyzer
```

---

### 🔗 jira-context

Extracts JIRA issue keys from PR context and enriches with ticket details via the Atlassian MCP. Scans all fetched content for prompt injection attempts. NON-BLOCKING.

**Features:**
- 🔑 Issue key extraction from branch name, PR title, and description
- 📋 Sub-task aware — fetches parent issue when needed
- 🛡️ Prompt injection detection across all fetched fields
- 🚦 NON-BLOCKING — returns partial data on failure, never aborts pipeline

**Installation:**
```bash
npx skills add https://github.com/cess15/skills --skill jira-context
```

---

## 🚀 Quick Start

### Install a Skill

```bash
# Install specific skill
npx skills add https://github.com/cess15/skills --skill security-compliance-review

# Or clone and install locally
git clone https://github.com/cess15/skills.git
cd skills
npx skills add . --skill security-compliance-review
```

### Use the Skill

Once installed, simply provide a PR diff to your AI agent:

```
Review this PR for security and compliance issues:
[paste your git diff here]
```

The agent will automatically use the skill to perform comprehensive security analysis.

---

## 📁 Repository Structure

```
skills/
├── README.md                          # This file
├── LICENSE                            # MIT License
├── .gitignore                        # Git ignore rules
├── skills/                           # Skills directory
│   ├── security-compliance-review/   # SAST & compliance skill
│   │   ├── README.md                # Skill documentation
│   │   ├── SKILL.md                 # Skill definition
│   │   └── examples/                # Usage examples
│   ├── alignment-analyzer/          # JIRA alignment skill
│   │   └── SKILL.md                 # Skill definition
│   └── jira-context/               # JIRA enrichment skill
│       └── SKILL.md                 # Skill definition
├── spec/                            # Specifications
│   └── agent-skills-spec.md        # Skills specification
└── template/                        # Skill template
    └── SKILL.md                    # Template for new skills
```

---

## 🛠️ Skills Coverage

### Security Analysis
- ✅ SQL Injection
- ✅ Command Injection
- ✅ XSS (Cross-Site Scripting)
- ✅ Authentication/Authorization Issues
- ✅ Secrets in Code
- ✅ Insecure Deserialization
- ✅ Security Misconfiguration
- ✅ SSRF (Server-Side Request Forgery)
- ✅ Path Traversal
- ✅ Weak Cryptography

### Compliance Standards
- ✅ **GDPR** - Personal data processing
- ✅ **HIPAA** - Protected Health Information
- ✅ **SOC2** - Access control & audit logging
- ✅ **PCI-DSS** - Payment card data security

### Supported Languages
Python • JavaScript/TypeScript • Java • Go • Ruby • PHP • C# • Rust • Swift

---

## 🎯 Design Philosophy

### Precision Over False Positives
Our skills focus on **real, exploitable vulnerabilities** rather than theoretical issues. We prioritize:
- Actionable findings with specific remediation steps
- Exploitability assessment from attacker perspective
- Minimal noise and false positives
- Language-agnostic pattern detection

### Developer-Friendly
- Clear, concise output
- Specific code examples
- Realistic exploit scenarios
- Prioritized recommendations

---

## 🤝 Contributing

Contributions are welcome! Whether you want to:
- Add new security patterns
- Improve detection accuracy
- Add new compliance standards
- Create new skills
- Fix bugs or improve documentation

Please feel free to submit a Pull Request or open an Issue.

### Adding a New Skill

1. Copy the template from `template/SKILL.md`
2. Create your skill in `skills/your-skill-name/`
3. Add examples and documentation
4. Test thoroughly
5. Submit a PR

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/cess15/skills/issues)
- **Discussions**: [GitHub Discussions](https://github.com/cess15/skills/discussions)

---

## 🙏 Acknowledgments

Built for the [Skills.sh](https://skills.sh) AI agent ecosystem.

---

## 🔗 Related Projects

- [Skills Documentation](https://skills.sh)
- [AI Agent Best Practices](https://github.com/anthropics/anthropic-cookbook)

---

**Made with ❤️ for secure software development**