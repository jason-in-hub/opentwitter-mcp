# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is `opentwitter-mcp`, a Python MCP (Model Context Protocol) server that exposes 8 Twitter/X data tools via the 6551 REST API. It is a single-package Python project with no frontend, no database, and no Docker infrastructure.

### Running the MCP server

The server uses stdio transport (not HTTP). To run it:

```bash
TWITTER_TOKEN=<token> uv run opentwitter-mcp
```

The `TWITTER_TOKEN` env var is **required** — the server raises `ValueError` at import time if it is missing. Get a token from https://6551.io/mcp. You can also set `TWITTER_API_BASE` and `TWITTER_MAX_ROWS` (see `README.md`).

### Testing with MCP Inspector

```bash
TWITTER_TOKEN=<token> npx @modelcontextprotocol/inspector uv run opentwitter-mcp
```

This starts a web UI on port 6274 (proxy on 6277). Use the session token printed to the terminal for authentication.

### Key caveats

- **No test suite exists** in this codebase. There are no unit or integration tests to run.
- **No linter configuration** is included (no ruff, flake8, mypy, or pyright config). You can run `uv run python -m py_compile src/opentwitter_mcp/server.py` to syntax-check files.
- `uv` is installed via `pip install uv` and lives at `/home/ubuntu/.local/bin/uv`. Ensure `PATH` includes this directory.
- The `config.py` module is imported eagerly — any import of `opentwitter_mcp` sub-modules triggers the token check. Always set `TWITTER_TOKEN` before importing.
- Build system is Hatchling; `uv sync` installs the package in editable mode along with all dependencies.
