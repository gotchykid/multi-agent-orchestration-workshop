# Setup — Install opencode (do this the night before)

**opencode** is the terminal AI coding agent you'll steer all workshop. You describe
what you want; it writes and edits the CrewAI code. This worksheet gets it installed,
authenticated, and proven working — **before** the session, not during it.

⏱️ ~10–15 minutes. 🆘 If you get stuck, come to the **office hours the evening before**.

> 📌 **Source of truth:** opencode updates often. If a command here looks different
> from what you see, trust the official docs at **https://opencode.ai/docs** — the
> *shape* of these steps (install → authenticate → smoke-test) stays the same.

---

## Step 0 — What you need first

| Need | Check |
|---|---|
| A terminal | macOS: **Terminal** or **iTerm**. Windows: **WSL2** (recommended) or **Git Bash**. Linux: any. |
| Node.js (only for the npm install path) | `node --version` → v18+ |
| An API key / login for a model provider | Whatever your **instructor specifies** (e.g. Anthropic or OpenAI). |

> 🪟 **Windows users:** opencode is a terminal (TUI) app and is happiest under **WSL2**
> (Windows Subsystem for Linux). If you don't have WSL, install it first
> (`wsl --install` in an admin PowerShell, then reboot) and run everything below
> *inside* the WSL/Ubuntu terminal. Git Bash works as a fallback.

---

## Step 1 — Install

Pick **one** method. The install script is the most reliable.

### Option A — Install script (macOS / Linux / WSL) ✅ recommended
```bash
curl -fsSL https://opencode.ai/install | bash
```

### Option B — npm (any OS with Node 18+)
```bash
npm install -g opencode-ai
```

### Option C — Homebrew (macOS / Linux)
```bash
brew install sst/tap/opencode
```

⚠️ **`PATH` warning after install?** The script may say opencode was installed to a
folder that isn't on your `PATH` (e.g. `~/.opencode/bin`). Follow the line it prints to
add it — usually appending an `export PATH=...` line to your `~/.zshrc` (macOS) or
`~/.bashrc` (Linux/WSL), then opening a **new** terminal.

---

## Step 2 — Start it up

That's it for installation. To launch opencode, just type:

```bash
opencode
```

The opencode TUI should open. ✅ **If it opens, you're ready for the workshop.**

---

## ✅ Setup checkpoint

- [ ] `opencode --version` prints a version number.
- [ ] Typing `opencode` opens the TUI.

If both pass, you're done. Next: **`SETUP-crewai.md`**.

---

## Stuck? Common failures

| Symptom | Likely cause | Fix |
|---|---|---|
| `opencode: command not found` | Install dir not on `PATH` | Re-read Step 1's PATH note; open a **new** terminal |
| TUI looks broken / garbled (Windows) | Running in plain CMD/PowerShell | Use **WSL2** or Git Bash instead |
| Auth key rejected | Typo, or key has no credit/quota | Re-copy the key; confirm the account is active |

> Bring **any** red checkbox to the office hours the evening before — not the morning of.
