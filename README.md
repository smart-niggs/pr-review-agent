# codepass

Single-pass PR review plugin for Claude Code. Posts inline GitHub comments, checks Copilot suggestions, validates test plans, and auto-determines review disposition — all in one context window (~40-60K tokens).

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- [`gh` CLI](https://cli.github.com/) installed and authenticated (`gh auth login`)

## Installation

```bash
claude plugin marketplace add smart-niggs/codepass
claude plugin install codepass
```

Or for local development:

```bash
claude --plugin-dir /path/to/codepass
```

## Usage

From any git repository with a GitHub remote:

```bash
# Review a PR by number
/codepass 219

# Review a PR by URL
/codepass https://github.com/org/repo/pull/219

# Analyze without posting (dry run)
/codepass 219 --dry-run
```

## Features

### 8-Dimension Analysis
1. **Security** — Token/secret exposure, auth bypass, injection vectors
2. **CLAUDE.md Compliance** — Convention violations with specific rule citations
3. **Bug Risk** — Logic errors, null handling, race conditions, silent failures
4. **PII / Data Exposure** — Logged PII, sensitive fields in responses
5. **Error Handling** — Framework-specific exception patterns, swallowed errors
6. **Convention Adherence** — Naming, imports, DTOs, module rules
7. **Test Plan Validation** — PR description test plan vs actual implementation
8. **Bot Comment Triage** — Copilot/bot comment validity, resolution status, overlap with findings

### Smart Disposition
- **APPROVE** — No medium+ findings, all bot comments resolved, test plan covered
- **REQUEST_CHANGES** — Any high/critical findings
- **COMMENT** — Medium findings or unresolved bot comments

### Selective Posting
After analysis, choose which findings to post:
- `all` — Post everything
- `1,2,5` — Post specific findings by number
- `critical+high` — Post by severity threshold
- `none` — Skip posting

### Token Efficient
| Approach | Tokens | Agents |
|----------|--------|--------|
| **codepass** | **~40-60K** | **0** |
| /code-review plugin | ~150-200K | 8+ |
| /pr-review-toolkit | ~200-300K | 6 |

## How It Works

1. Parses PR number from your input
2. Fetches PR metadata, diff, and review threads (3 parallel API calls)
3. Reads local CLAUDE.md conventions and changed files
4. Analyzes across 8 dimensions in a single pass
5. Presents structured findings with severity and confidence scores
6. Asks which findings to post and whether to override disposition
7. Posts review with inline comments to GitHub

## Configuration

No configuration needed. The plugin:
- Detects repo owner/name from your current directory (`gh repo view`)
- Uses your `gh` CLI authentication for GitHub API calls
- Reads `CLAUDE.md` files dynamically from whatever repo you're in
- Posts reviews under your own GitHub account

## License

MIT
