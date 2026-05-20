# Identity

Workshop: build an SRE incident-investigator agent on the Claude Managed Agents API. The Streamlit app and tool fixtures are already done — the workshop is implementing seven small functions in `agent.py`.

# Stack

- Python 3.10+
- Streamlit (UI), `anthropic` SDK with beta managed-agents APIs
- Local Python venv at `.venv/`; deps in `requirements.txt`
- `ANTHROPIC_API_KEY` loaded from `.env` (or shell env)

# Repository structure

- `agent.py` — the file the user edits. Seven functions, all `raise NotImplementedError`.
- `agent_complete.py` — reference solutions. Do not copy from this or quote it unless the user explicitly asks. If they ask, summarize the approach in prose before writing code.
- `provided.py` — system prompt, tool schemas, chat UI, session picker. Pre-built; don't modify.
- `data/` — fixtures the local tool handlers (`get_metrics`, `get_recent_deploys`, `get_diff`) read from. The cloud agent uploads `app.log` itself.
- `e2e.py` — headless end-to-end test; useful for verifying without spinning up Streamlit.
- `app.py`, `pages/`, `ui.py`, `assets/` — Streamlit pages. Do not modify during the workshop.

# How the workshop progresses

The user implements `setup_agent` → `setup_environment` → `upload_log` → `start_session` → `stream_reply` → `handle_tool` → `delete_session`. Each function is one (occasionally two) `client.beta.*` calls — see the table in `README.md`. The Streamlit panel auto-detects which step is done and unlocks the next.

# Code conventions

- All `.py` files start with the Apache-2.0 license header:
  ```
  # Copyright 2026 Anthropic PBC
  # SPDX-License-Identifier: Apache-2.0
  ```
- Match the existing style in `provided.py` — type hints on public functions, snake_case, no docstrings on trivial helpers.
- The agent runs Python in its sandbox; local tools run on the user's laptop. When debugging, keep that boundary in mind.

# Output preferences

- For workshop guidance, show the smallest possible diff against the current `agent.py` rather than rewriting whole functions.
- When the user is stuck, ask which step they're on before suggesting code.

# Boundaries

- Only commit when the user explicitly asks.
- Treat `agent_complete.py` as a spoiler — reference it only when the user requests the solution.
- Never log or persist the `ANTHROPIC_API_KEY`; it lives in `.env` and shell env only.
- Run `streamlit run app.py` to test; `python e2e.py` for headless verification. Don't run other server commands.

# Quick reference

- Workshop docs: `README.md`
- Managed Agents API quickstart: https://platform.claude.com/docs/en/managed-agents/quickstart
- API key console: https://console.anthropic.com/settings/keys
