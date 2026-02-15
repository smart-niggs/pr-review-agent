# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x     | Yes       |

## How codepass Uses Your Credentials

codepass delegates all GitHub API calls to the `gh` CLI. It never stores, transmits, or logs your GitHub token directly. Your review is posted under your own GitHub account using your existing `gh auth` session.

## Reporting a Vulnerability

**Do not open a public issue for security vulnerabilities.**

Email **smartniggs@gmail.com** with:

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

You can expect an initial response within 72 hours.

## Security Best Practices for Users

- Keep `gh` CLI updated (`gh upgrade`)
- Use fine-grained personal access tokens instead of classic tokens
- Review plugin source code before installing: `gh repo view smart-niggs/codepass --web`
- Use `--dry-run` to preview review output before posting to GitHub
