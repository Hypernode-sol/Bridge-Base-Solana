# Security Policy

## Supported Versions

We release patches for security vulnerabilities in the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

The Hypernode team takes security issues seriously. We appreciate your efforts to responsibly disclose your findings.

### How to Report

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them via email to:

**contact@hypernodesolana.org**

Include the following information:

- Type of issue (e.g. buffer overflow, SQL injection, cross-chain replay attack, etc.)
- Full paths of source file(s) related to the manifestation of the issue
- The location of the affected source code (tag/branch/commit or direct URL)
- Step-by-step instructions to reproduce the issue
- Proof-of-concept or exploit code (if possible)
- Impact of the issue, including how an attacker might exploit it

### What to Expect

After you submit a report, we will:

1. **Acknowledge receipt** within 48 hours
2. **Provide an estimated timeline** for a fix within 7 days
3. **Notify you** when the vulnerability is fixed
4. **Publicly acknowledge your contribution** (if you wish)

### Bug Bounty Program

We are planning to launch a bug bounty program in Q2 2025. Details will be announced on our website and GitHub.

### Security Best Practices

When using the bridge:

- **Never commit private keys** to version control
- **Use hardware wallets** for production deployments
- **Verify contract addresses** before bridging assets
- **Start with small test amounts** before large transfers
- **Monitor bridge events** through our telemetry dashboard
- **Keep dependencies updated** regularly

### Audit Status

| Component | Status | Date | Auditor |
|-----------|--------|------|---------|
| Base Contracts | Planned | Q3 2025 | TBD |
| Solana Program | Planned | Q3 2025 | TBD |
| Relayer System | Planned | Q4 2025 | TBD |

### Known Issues

We maintain a list of known issues and their status:

- None reported as of March 2025

### Security Contact

For security-related inquiries, please contact:

- Email: contact@hypernodesolana.org
- Response time: Within 48 hours
