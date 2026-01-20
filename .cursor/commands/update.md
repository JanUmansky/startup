# /update — Update an existing assistant command

Definitions:
- COMMANDS_DIR = the folder where command markdown files live.
- ROSTER_FILE = COMMANDS_DIR + "/_roster.md"
- Command file path = COMMANDS_DIR + "/<name>.md"

Goal:
Update an existing assistant’s mission/scope/rules/workflow based on the user’s input.

## Input
User message: "/update <name> <new specs>"
Example: "/update bob add OWASP checklist and enforce zod validation on all new routes"

## Reserved Names (Protected)
The following names are **system commands** and cannot be updated via /update:
- hire, fire, update, list, define, _roster

If the user attempts to update a reserved name, **reject the request** with a clear error:
> ❌ Cannot update `<name>` — this is a protected system command.
> Only hired assistants can be modified with /update.

## Steps
1) Parse assistant name + requested changes.
2) **Validate the name is not reserved:**
   - Check against reserved names: hire, fire, update, list, define, _roster
   - If reserved, reject immediately with the error message above
   - Do NOT proceed with modification
3) Open the command file and read current Mission/Scope/Rules.
4) Apply updates with minimal disruption:
   - Update Mission if needed
   - Adjust In scope / Out of scope
   - Extend checklists and operating rules
   - Update interfaces if target area changed
5) If the one-line mission changed materially, update the entry in ROSTER_FILE too.
6) Summarize the diff with structured output.

## Output Format

Structure the response with emojis and tables:

---

### 📝 Summary

> One sentence: what was updated.

Example: *Updated **dinesh** with OWASP security checklist.*

---

### 📁 Files Modified

| File | Location |
|------|----------|
| `dinesh.md` | `.cursor/commands/` |
| `_roster.md` | `.cursor/commands/` (if mission changed) |

---

### 🔄 Changes

| Section | Before | After |
|---------|--------|-------|
| Operating Rules | 4 items | 8 items (+OWASP checklist) |
| In Scope | validation, auth | validation, auth, security audits |

---

### ➕ Added

- ✅ OWASP Top 10 security checklist
- ✅ Zod validation enforcement
- ✅ SQL injection prevention rules

---

### ✅ Ready to Use

```
/dinesh <your task here>
```

---

## Guardrails
- Keep the assistant focused—don't turn it into a generalist.
- Prefer concrete checklists and rules over vague statements.
