# Contributing to codepass

Thanks for your interest in contributing. This guide covers how to report issues, suggest features, and submit changes.

## Reporting Bugs

Open a [GitHub issue](https://github.com/smart-niggs/codepass/issues/new?template=bug_report.yml) with:

- Steps to reproduce (PR number/URL, flags used)
- Expected vs actual behavior
- Claude Code version (`claude --version`)
- `gh` CLI version (`gh --version`)
- OS

## Suggesting Features

Open a [feature request](https://github.com/smart-niggs/codepass/issues/new?template=feature_request.yml). Include the use case and how you'd expect it to work.

## Development Setup

```bash
git clone https://github.com/smart-niggs/codepass
cd codepass
```

### Validate the plugin

```bash
claude plugin validate .
```

### Test against a real PR

```bash
# In any repo with an open PR:
/codepass:pr-review <PR-number> --dry-run
```

### Test cross-repo review

```bash
/codepass:pr-review https://github.com/owner/repo/pull/123 --dry-run
```

## Submitting Changes

1. Fork the repo and create a branch from `main`
2. Make your changes to `plugins/codepass/commands/pr-review.md` or other files
3. Run `claude plugin validate .` to ensure the plugin is valid
4. Test your changes against at least one real PR with `--dry-run`
5. Open a PR with a clear description of what changed and why

### What to include in your PR

- Description of the problem or feature
- How you tested it (which PRs, what scenarios)
- Any edge cases you considered

### Style

- `pr-review.md` is a prompt file — write clear, unambiguous instructions
- Keep bash examples copy-pasteable
- Severity/confidence thresholds should be justified

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
