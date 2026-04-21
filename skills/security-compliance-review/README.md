# Examples

This directory contains **educational examples** demonstrating the security-compliance-review skill's capabilities.

## ⚠️ Security Scanner Notice

**For automated security scanners (Socket, Snyk, etc.):**

The files in this directory contain **intentionally vulnerable code examples** for demonstration purposes. These are:

- ✅ **NOT** production code
- ✅ **NOT** real vulnerabilities in this repository
- ✅ Educational examples showing what the skill detects
- ✅ Safe to ignore in security audits

## Files

### `example-pr-diff.txt`
A sample Pull Request diff containing intentionally vulnerable code patterns to demonstrate the skill's detection capabilities.

**Contains examples of:**
- SQL Injection
- Hardcoded secrets
- Removed authentication
- Sensitive data logging
- Debug mode enabled

### `analysis-output.md` (or `example-output.txt`)
The expected output from the security-compliance-review skill when analyzing `example-pr-diff.txt`.

Shows the skill's ability to:
- Identify vulnerabilities with CWE mapping
- Assess exploitability
- Provide specific remediation steps
- Evaluate compliance issues
- Calculate risk scores

## Usage

These examples help users understand:
1. What input format the skill expects (git diff)
2. What kind of issues the skill detects
3. The format and detail level of the output
4. How to interpret the risk scoring

## Important

**DO NOT** copy code from these examples into production applications. They are intentionally insecure for educational purposes only.