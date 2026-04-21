# Security Compliance Review

> AI-powered SAST and compliance analysis for Pull Request diffs

Advanced security and compliance analysis skill for AI code review agents. Performs static application security testing (SAST) and regulatory compliance checks on Pull Request diffs across any programming language or framework.

## ✨ Features

- 🔍 **Advanced SAST Analysis** - Detects real vulnerabilities with minimal false positives
- 🌐 **Language-Agnostic** - Works with Python, JavaScript, Java, Go, Ruby, PHP, C#, and more
- 📋 **Compliance Checks** - GDPR, HIPAA, SOC2, PCI-DSS validation
- 🎯 **Diff-Only Analysis** - Focuses only on changed code for precision
- 💥 **Exploitability Assessment** - Evaluates real-world attack scenarios
- 🏷️ **CWE Mapping** - Maps vulnerabilities to Common Weakness Enumeration
- 📊 **Risk Scoring** - Numeric risk assessment (0-10 scale)
- 🚫 **Zero False Positives Philosophy** - Precision over verbosity

## 🚀 Installation

```bash
npx skills add https://github.com/cess15/skills --skill security-compliance-review
```

## 📖 Usage

Once installed, the skill automatically activates when you provide a Pull Request diff or git diff to your AI agent.

### Example: Analyze a PR

```
Review this PR for security issues:

diff --git a/api/users.py b/api/users.py
index 1234567..abcdefg 100644
--- a/api/users.py
+++ b/api/users.py
@@ -15,7 +15,7 @@ class UserAPI:
     def get_user(self, user_id):
-        cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
+        cursor.execute("SELECT * FROM users WHERE id = " + user_id)
         return cursor.fetchone()
```

### Example Output

The skill will provide:

**🔴 Security Issues**
- Severity classification (Critical/High/Medium/Low)
- CWE IDs
- Exploit scenarios
- Specific remediation steps

**🟡 Compliance Issues**
- Regulatory standard violations
- Risk assessment
- Compliance recommendations

**🟢 Summary**
- Overall risk level
- Risk score (0-10)
- Prioritized action items

See [examples/analysis-output.md](examples/analysis-output.md) for a complete example.

## 🔐 Security Coverage

### OWASP Top 10
- Injection (SQL, Command, Code)
- Broken Authentication & Authorization
- Sensitive Data Exposure
- Security Misconfiguration
- Insecure Deserialization
- XSS (Cross-Site Scripting)
- SSRF (Server-Side Request Forgery)
- Path Traversal
- Weak Cryptography
- Vulnerable Components

### Compliance Standards
- **GDPR** - Personal data processing, consent, data minimization
- **HIPAA** - PHI encryption, access controls, audit logging
- **SOC2** - Security controls, monitoring, incident response
- **PCI-DSS** - Cardholder data protection, encryption

## 🛠️ Supported Languages

Python • JavaScript/TypeScript • Java • Go • Ruby • PHP • C# • Rust • Swift • and more

## 🎯 Design Philosophy

### Attacker Mindset
Evaluates vulnerabilities from an attacker's perspective with realistic exploit scenarios and business impact assessment.

### Precision Over Noise
- Reports only real, exploitable vulnerabilities
- Minimal false positives
- No generic security advice without specific risk
- Prefers zero findings over incorrect findings

### Actionable Recommendations
- Specific code fixes
- Step-by-step remediation
- Prioritized by severity and exploitability

## 📋 Detection Heuristics

| Pattern | Risk | Example |
|---------|------|---------|
| User input → SQL/shell/eval | Critical | `cursor.execute("... " + user_input)` |
| Hardcoded credentials | Critical | `API_KEY = "sk_live_..."` |
| Removed auth checks | Critical | Deleted `@login_required` |
| Unsafe deserialization | High | `pickle.loads(untrusted_data)` |
| PII in logs | High | `logger.info(f"password: {pwd}")` |
| Debug mode in production | High | `DEBUG = True` |
| Weak randomness | Medium | `token = random.randint()` |

## 🤝 Contributing

Found a pattern we're missing? Want to improve detection? Contributions welcome!

## 📄 License

MIT License

## 🔗 Links

- [Main Repository](https://github.com/cess15/skills)
- [Report Issues](https://github.com/cess15/skills/issues)
- [Full Specification](../../spec/agent-skills-spec.md)

---

**Part of the [cess15/skills](https://github.com/cess15/skills) collection**