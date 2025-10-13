## Contributing to Zen MCP Server

Thanks for improving Zen MCP! This guide covers the basics to get you productive quickly and avoid common pitfalls.

### 1) Local setup
- Python 3.10+ (3.13 recommended)
- uv (https://docs.astral.sh/uv/)
- Make sure sibling pheno-sdk is checked out at ../pheno-sdk when working with local SDKs

### 2) Install pre-commit hooks (required)
To enforce consistent quality gates locally, install pre-commit hooks once per clone:

```bash
pip install pre-commit  # or: uv tool install pre-commit
pre-commit install      # installs hooks for this repo
```

Run hooks on all files before your first commit:
```bash
pre-commit run -a
```

What the hooks enforce:
- Hardcoded secret guard (on newly ADDED lines)
- Large-file guard: blocks files > 50MB from being committed
- Format/lint (Black, isort, Ruff) where applicable

Tip: If a large artifact needs to exist (e.g., binaries), use Git LFS and add explicit path rules. Avoid committing caches (.next/, node_modules/) and archives (.docs_archive/).

### 3) Commit & PR guidelines
- Small, focused commits with clear messages
- Tests: add/adjust unit tests for any code changes; run the smallest relevant test scope locally
- Documentation: update relevant docs in `docs/` and crosslink code paths where useful
- CI will fail PRs introducing files > 50MB; fix by removing large files from the commit or using LFS appropriately

### 4) Running tests
- FastMCP unit subset (example):
  ```bash
  uv run -p 3.13 pytest -q tests/unit/test_fastmcp_agent_client_*.py
  ```
- Smoke tests (no OAuth):
  ```bash
  ZEN_DISABLE_OAUTH_TESTS=1 MCP_TEST_TYPE=dry \
  uv run -p 3.13 pytest -q smoke/test_cli_zen_entrypoint.py
  ```

### 5) Working with pheno-sdk locally
If you are developing SDK components alongside this repo:
- Build wheels in ../pheno-sdk/<package>/ and install into your env with `uv run -p 3.13 pip install path/to/dist.whl`
- Or temporarily expose sources via `export PYTHONPATH=../pheno-sdk:$PYTHONPATH` (for local testing only)

### 6) Reporting issues / proposing changes
- Use GitHub issues with clear reproduction steps
- For larger docs/code reorganizations, open an RFC/plan issue first; we stage multi-file changes in small, reviewable PRs.

