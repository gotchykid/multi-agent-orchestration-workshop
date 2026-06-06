# Setup — Install CrewAI (do this the night before)

**CrewAI** is the Python framework you'll build your crew with. This worksheet installs
Python + `uv` + the **CrewAI CLI**, scaffolds your project, and puts your API keys in
place — **before** the session.

⏱️ ~10–15 minutes. 🆘 Stuck? Come to the **office hours the evening before**.

> 📌 **Source of truth:** if a command drifts from what you see, check
> **https://docs.crewai.com**. The shape stays the same: Python → uv → CLI → scaffold → keys → smoke-test.

---

## Step 0 — What you need first

| Need | Check / get it |
|---|---|
| **Python 3.10–3.13** | `python3 --version` (CrewAI needs `>=3.10,<3.14`) |
| **uv** (Python package manager) | install per **https://astral.sh/uv**, then `uv --version` |
| **API key for the crew's LLM** | from your **instructor** (see Step 4) |
| **API key for web search** | e.g. SerperDev free tier (see Step 4) |

### Install `uv` if you don't have it
```bash
# macOS / Linux / WSL
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
Open a **new** terminal afterwards, then confirm: `uv --version`.

> 🪟 **Windows:** you can use native PowerShell here, *or* WSL2. If you're already using
> WSL2 for opencode, just do everything in the same WSL terminal — simpler.

---

## Step 1 — Install the CrewAI CLI

CrewAI ships its command-line tool as **`crewai-cli`**. Install it as a global `uv` tool —
it lives in its own isolated environment and gives you the `crewai` command for scaffolding,
installing, and running crews.

```bash
uv tool install crewai-cli
```

Open a **new** terminal, then confirm:
```bash
crewai --version
```

> ⬆️ **Upgrade later:** `uv tool install crewai-cli --upgrade`.
> 🩹 **`crewai: command not found`?** Run `uv tool update-shell`, then reopen the terminal.

---

## Step 2 — Scaffold the workshop project

```bash
crewai create crew market_brief
cd market_brief
```

When prompted, **choose your LLM provider** (e.g. `1` for OpenAI) and accept the default
model — you can leave the API key blank for now; we set keys in Step 4.

> 🤖 **Prompt getting in the way?** Skip it entirely:
> `crewai create crew market_brief --provider openai --skip_provider`

This generates a standard CrewAI project. The parts you'll actually touch:

```
market_brief/
├─ pyproject.toml              # dependencies (crewai[tools]) + the `crewai run` script
└─ src/market_brief/
   ├─ main.py                  # entry point — sets the inputs (your topic)
   ├─ crew.py                  # agents, tasks, and the crew (the @CrewBase class)
   └─ config/
      ├─ agents.yaml           # WHO the agents are: role / goal / backstory
      └─ tasks.yaml            # WHAT they do: description / expected_output / agent
```

The big idea: **agents and tasks are described in YAML; `crew.py` wires them together.**
You'll point opencode at **this folder** during the workshop.

---

## Step 3 — Install dependencies

```bash
crewai install
```

This creates the project's virtual environment and installs `crewai` + `crewai-tools`.
⏳ First install pulls a fair number of dependencies — give it a minute or two.

**Verify it imported:**
```bash
uv run python -c "import crewai; print('crewai', crewai.__version__)"
uv run python -c "from crewai_tools import SerperDevTool; print('tools ok')"
```
Both should print without a traceback (you should see crewai 1.14 or newer). ✅

---

## Step 4 — Set your API keys

Create a file called `.env` **in the project root** (`market_brief/`). The scaffold doesn't
make one for you. **Fill in the values your instructor gives you.**

```bash
# .env  — the crew's runtime LLM (pick the ONE your instructor specified)
MODEL=gpt-4o-mini                     # OpenAI default
OPENAI_API_KEY=sk-...

# Gemini instead? Comment the two lines above and use:
# MODEL=gemini/gemini-1.5-flash
# GEMINI_API_KEY=...

# Web search tool
SERPER_API_KEY=...                    # free tier at https://serper.dev
```

CrewAI reads `MODEL` to pick the default LLM for every agent (you can also override it
per-agent later with an `llm:` line in `agents.yaml`).

| Your instructor said use… | `.env` lines | Where to get it |
|---|---|---|
| OpenAI | `MODEL=gpt-4o-mini` + `OPENAI_API_KEY` | platform.openai.com |
| Gemini | `MODEL=gemini/gemini-1.5-flash` + `GEMINI_API_KEY` | aistudio.google.com |
| Serper (web search) | `SERPER_API_KEY` | serper.dev (free tier, ~2,500 searches) |

⚠️ **Never commit `.env`.** The scaffold's `.gitignore` already ignores it — confirm with
`grep .env .gitignore` (if missing: `echo ".env" >> .gitignore`).
⚠️ This is the crew's *runtime* LLM — **separate** from the model opencode uses to write
code (see `SETUP-opencode.md`). You may have two different keys. That's normal.

> 🔑 **No Serper key?** That's fine — there's a no-API-key DuckDuckGo fallback in
> `BUILD-1.md`. You still need the LLM key, though.

---

## Step 5 — Smoke test (prove a crew can actually run)

The scaffold ships a tiny starter crew. Running it confirms your install **and** your LLM
key work together — the failure most people only discover mid-build.

```bash
crewai run
```

✅ **You see a verbose log and a `report.md` file appears? Your CrewAI + LLM key work.**

⚠️ If it errors with an **authentication** message, your LLM key is wrong or unset — fix
Step 4. This is exactly the error that otherwise surfaces at minute 35 of the workshop.

You'll **replace this starter crew with your own** in `BUILD-1.md` — but **keep the project
folder**; you'll build in it.

---

## ✅ Setup checkpoint

- [ ] `crewai --version` prints a version.
- [ ] `crewai install` finished without errors.
- [ ] `uv run python -c "import crewai"` runs clean.
- [ ] `.env` has your LLM key (and search key, unless using the fallback).
- [ ] `crewai run` produced a `report.md` — LLM key confirmed working.

All five pass → you're fully set. See you at the workshop with `BUILD-1.md`.

---

## Stuck? Common failures

| Symptom | Likely cause | Fix |
|---|---|---|
| `crewai: command not found` | CLI not on PATH | `uv tool update-shell`, re-open the terminal; re-check Step 1 |
| `crewai create` hangs on a provider prompt | Waiting for input | Pick a number, or use `--provider openai --skip_provider` |
| `ModuleNotFoundError: crewai` | Deps not installed / not in project | `crewai install` inside the project; run via `uv run` |
| `ModuleNotFoundError: crewai_tools` | Tools extra missing | `crewai install` again (pyproject pins `crewai[tools]`) |
| Build error mentioning Python version | Python too old/new | Install Python 3.10–3.13 (CrewAI needs `>=3.10,<3.14`) |
| `AuthenticationError` on `crewai run` | LLM key missing/typo | Fix `.env`; confirm `MODEL` + the matching `*_API_KEY` |
| `crewai run` ignores `.env` | `.env` not in the project root | It must sit next to `pyproject.toml` |

> Bring **any** red checkbox to the office hours the evening before.
