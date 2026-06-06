# Setup — Install CrewAI (do this the night before)

**CrewAI** is the Python framework you'll build your crew with. This worksheet installs
Python + `uv` + the **CrewAI CLI** and scaffolds your project — **before** the session.

⏱️ ~10–15 minutes. 🆘 Stuck? Come to the **office hours the evening before**.

> 📌 **Source of truth:** if a command drifts from what you see, check
> **https://docs.crewai.com**. The shape stays the same: Python → uv → CLI → scaffold.

---

## Step 0 — What you need first

| Need | Check / get it |
|---|---|
| **Python 3.10–3.13** | `python3 --version` (CrewAI needs `>=3.10,<3.14`) |
| **uv** (Python package manager) | install per **https://astral.sh/uv**, then `uv --version` |

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
model — you can leave the API key blank for now; you'll set keys during the workshop.

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

## ✅ Setup checkpoint

- [ ] `crewai --version` prints a version.
- [ ] `crewai create crew market_brief` scaffolded the project folder.

Both pass → you're fully set. See you at the workshop with `BUILD-1.md`.

---

## Stuck? Common failures

| Symptom | Likely cause | Fix |
|---|---|---|
| `crewai: command not found` | CLI not on PATH | `uv tool update-shell`, re-open the terminal; re-check Step 1 |
| `crewai create` hangs on a provider prompt | Waiting for input | Pick a number, or use `--provider openai --skip_provider` |
| Build error mentioning Python version | Python too old/new | Install Python 3.10–3.13 (CrewAI needs `>=3.10,<3.14`) |

> Bring **any** red checkbox to the office hours the evening before.
