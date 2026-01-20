# /list — Show active assistants (slash commands)

Definitions:
- COMMANDS_DIR = the folder where command markdown files live (this file is already in it).
- ROSTER_FILE = COMMANDS_DIR + "/_roster.md"

Goal:
Display the active assistants and what they do.

## Output requirements
- Print a readable list of assistants with:
  - Command name (e.g., /bob)
  - One-line mission (from the assistant file, if possible)
  - Focus/scope highlights (bullets, short)
- Separate "system commands" (hire/fire/update/list) from "hired assistants".

## Steps
1) Identify system commands:
   - Treat these as system commands: hire, fire, update, list, define
   - Do not include them in "Active Assistants".

2) Try roster-first:
   - If ROSTER_FILE exists and contains an “Active Assistants” section, parse it and list assistants in that section.

3) Validate roster entries:
   - For each assistant listed in roster, confirm COMMANDS_DIR + "/<name>.md" exists.
   - If missing, mark it as “missing file”.

4) If roster missing or empty:
   - Fallback: scan COMMANDS_DIR for `*.md` files (excluding `_roster.md` and system commands)
   - Consider each remaining file an assistant command.

5) Enrich from assistant files:
   - For each assistant command file, attempt to extract:
     - Mission: first paragraph under “## Mission”
     - Optional: first 3 bullets under “### In scope”
   - If sections are missing, show “(no mission found)” etc.

6) Present results:
   - First: System commands available (always show).
   - Then: Active assistants.

## Output Format

Structure the response with emojis and tables:

---

### 🛠️ System Commands

| Command | Description |
|---------|-------------|
| `/hire` | Create a new specialist agent |
| `/fire` | Remove an agent |
| `/update` | Modify an agent's scope |
| `/list` | Show this overview |
| `/define` | Create project-wide rules |

---

### 👥 Your Team

| Agent | Mission | Focus |
|-------|---------|-------|
| 🧑‍💻 `/dinesh` | Backend API Guardian | validation, auth, routes |
| 🔧 `/gilfoyle` | DevOps Specialist | infra, CI/CD, monitoring |

---

### 📁 Files

| Agent | File |
|-------|------|
| dinesh | `.cursor/commands/dinesh.md` |
| gilfoyle | `.cursor/commands/gilfoyle.md` |

---

### ⚠️ Issues (if any)

| Agent | Status |
|-------|--------|
| ❌ `/xyz` | Missing file — listed in roster but not found |

---

### 📜 Project Rules

| File | Scope |
|------|-------|
| `conventions.mdc` | `alwaysApply: true` |
| `python.mdc` | `globs: *.py` |

**Location:** `.cursor/rules/`

---
