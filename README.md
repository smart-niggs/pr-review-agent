# codepass

Single-pass PR review plugin for Claude Code. Posts inline GitHub comments, checks Copilot suggestions, validates test plans, and auto-determines review disposition.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- [`gh` CLI](https://cli.github.com/) installed and authenticated (`gh auth login`)

## Installation

```bash
claude plugin marketplace add smart-niggs/codepass
claude plugin install codepass
```

## Usage

Open Claude Code inside any git repository with a GitHub remote:

```bash
# Start Claude Code in your repo
claude
```

Then use the slash command inside Claude Code:

```bash
# Review a PR by number
/codepass:pr-review 219

# Review a PR by URL
/codepass:pr-review https://github.com/org/repo/pull/219

# Analyze without posting (dry run)
/codepass:pr-review 219 --dry-run
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

## How It Compares

| Tool | Cost | Context | Convention Awareness | Structured Analysis | Disposition Logic | Status |
|------|------|---------|---------------------|--------------------|--------------------|--------|
| **codepass** | **Free** | **Changed files + diff + metadata (single pass)** | **Yes (CLAUDE.md)** | **8 dimensions** | **Yes** | **Active** |
| [ai-pr-reviewer](https://github.com/coderabbitai/ai-pr-reviewer) | Free (BYOK) | Diff chunks (incremental) | No | None | No | Unmaintained |
| [ai-codereviewer](https://github.com/villesau/ai-codereviewer) | Free (BYOK) | Diff chunks (one-shot) | No | None | No | Unmaintained |
| [CodeRabbit](https://coderabbit.ai) | $12-24/dev/mo | Full repo clone + code graph + semantic index | Config files | Multi-linter (40+) | Yes | Active |
| [Copilot PR Review](https://docs.github.com/en/copilot) | $10-39/dev/mo | Diff + agentic file retrieval | `.github/copilot-instructions.md` | Risk scoring | No | Active |
| [Qodo Merge](https://github.com/qodo-ai/pr-agent) | Free (OSS) / $30-45/dev/mo | Diff + token budgeting + chunking | Config files (paid) | Single LLM call | No | Active |

**What makes codepass different:**
- **Convention-first** — reads your `CLAUDE.md` and cites specific rule violations. No other tool does this natively.
- **Single-pass, zero agents** — one context window, no agent spawning.
- **You control what gets posted** — selective posting by finding number or severity threshold. No auto-spam.

## How It Works

1. Verifies `gh` authentication
2. Parses PR number or URL, resolves repo with `--repo` for all API calls
3. Fetches PR metadata, diff, and review threads (3 parallel API calls)
4. Warns if PR has 50+ changed files
5. Reads CLAUDE.md conventions from the **base branch** (not the PR head)
6. Fetches changed files — uses GitHub API for fork PRs, skips binaries/lockfiles
7. Analyzes across 8 dimensions in a single pass
8. Presents structured findings with severity and confidence scores
9. Asks which findings to post and whether to override disposition
10. Posts review with inline comments pinned to the specific commit SHA

## Configuration

No configuration needed — but works best when your repo has a `CLAUDE.md` with project conventions. Without one, codepass skips convention checks and still reviews the other 7 dimensions.

The plugin:
- Detects repo from your current directory or from the provided URL
- Uses your `gh` CLI authentication for all GitHub API calls
- Reads `CLAUDE.md` files from the base branch of whatever repo the PR targets
- Posts reviews under your own GitHub account
- Pins reviews to the PR's head commit SHA to avoid stale line comments

## Limitations

- **Review threads**: Fetches up to 100 threads with 20 comments each
- **Binary/lock files**: Automatically skipped
- **Large PRs**: Warns at 50+ files. Files prioritized by diff size.
- **Fork PRs**: Requires the fork repo to be accessible to your `gh` token
- **Duplicate reviews**: Running twice creates a second review (GitHub doesn't support editing reviews)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

See [SECURITY.md](SECURITY.md).

## License

[MIT](LICENSE)
