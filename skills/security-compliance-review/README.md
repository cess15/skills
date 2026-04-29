# security-compliance-review

Advanced SAST and compliance analysis for Pull Request diffs. Identifies real, exploitable vulnerabilities and regulatory compliance violations by analyzing only changed code. Multi-language.

## Features

- OWASP Top 10 detection with CWE mapping
- Compliance checks: GDPR, HIPAA, SOC2, PCI-DSS
- Diff-only analysis — `-` lines used only for data-flow context, never as findings
- All Medium+ findings reported with exploitability rationale
- Exploitability assessment from attacker perspective
- Risk scoring (0–10 scale)

## Installation

```bash
npx skills add https://github.com/cess15/skills --skill security-compliance-review
```

## Input

Accepts a PR diff (git diff format) plus optional PR metadata:

```
PR Title: Add user login endpoint
PR Description: Implements JWT-based auth

[git diff content]
```

## Output structure

```
### 🔴 Security Issues
Issue 1: [Title]
- Severity: Critical / High / Medium / Low
- CWE: CWE-XXX
- File + line
- Code snippet
- Exploit scenario
- Recommendation

### 🟡 Compliance Issues
Issue 1: [Standard] - [Title]
- Standard: GDPR / HIPAA / SOC2 / PCI-DSS
- Violation + risk + recommendation

### 🟢 Summary
- Overall Risk Level
- Risk Score: X/10
- Issues count
- Key Risks (max 3)
- Recommended Actions (max 3)
```

## Risk scoring

```
Score = Σ(weight × count)  — capped at 10

Critical: ×3.0 | High: ×2.0 | Medium: ×1.0 | Low: ×0.3
```

## Supported languages

Python · JavaScript/TypeScript · Java · Go · Ruby · PHP · C# · and others (semantic pattern fallback for unlisted languages)

## Detection categories

| Category | Examples |
|---|---|
| Injection | SQL, Command, Code, LDAP, Template |
| Auth | Missing auth checks, hardcoded credentials, weak crypto, JWT misuse |
| Secrets | API keys, passwords, tokens in code |
| Misconfiguration | DEBUG=True, permissive CORS, exposed admin routes |
| Deserialization | pickle.loads, yaml.load on untrusted data, XXE |
| SSRF | User-controlled URLs in fetch/request |
| XSS | innerHTML, dangerouslySetInnerHTML, reflected input |
| Path Traversal | User input in file paths |
| Weak Randomness | random() for tokens or passwords |
