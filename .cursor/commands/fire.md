# /fire — Remove an existing assistant command

Definitions:
- COMMANDS_DIR = the folder where command markdown files live.
- ROSTER_FILE = COMMANDS_DIR + "/_roster.md"
- Command file path = COMMANDS_DIR + "/<name>.md"

Goal:
Remove an assistant command file and unregister it from ROSTER_FILE.

## Input
User message: "/fire <name>" (e.g., "/fire bob" or "/fire api-guardian")

## Reserved Names (Protected)
The following names are **system commands** and cannot be fired:
- hire, fire, update, list, define, _roster

If the user attempts to fire a reserved name, **reject the request** with a clear error:
> ❌ Cannot fire `<name>` — this is a protected system command.
> Only hired assistants can be removed with /fire.

## Steps
1) Extract assistant name.
2) **Validate the name is not reserved:**
   - Check against reserved names: hire, fire, update, list, define, _roster
   - If reserved, reject immediately with the error message above
   - Do NOT proceed with deletion
3) Resolve the command file:
   - Prefer exact match COMMANDS_DIR/<name>.md
   - If not found, search within ROSTER_FILE for close matches and select the best candidate.
4) Delete the assistant command file.
5) Update ROSTER_FILE:
   - Remove the assistant's entry under "Active Assistants".
6) Report what was removed with structured output.

## Output Format

Structure the response with emojis and tables:

---

### 🔥 Summary

> One sentence: who was fired.

Example: *Removed **dinesh** from your team.*

---

### 📁 Changes

| Action | File | Location |
|--------|------|----------|
| 🗑️ Deleted | `dinesh.md` | `.cursor/commands/` |
| 📝 Updated | `_roster.md` | `.cursor/commands/` |

---

### 👥 Remaining Team

| Agent | Mission |
|-------|---------|
| 🔧 `/gilfoyle` | DevOps Specialist |

*Use `/list` to see full team details.*

---

## Safety
- Only delete within COMMANDS_DIR unless explicitly asked.
