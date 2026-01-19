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

## Steps
1) Parse assistant name + requested changes.
2) Open the command file and read current Mission/Scope/Rules.
3) Apply updates with minimal disruption:
   - Update Mission if needed
   - Adjust In scope / Out of scope
   - Extend checklists and operating rules
   - Update interfaces if target area changed
4) If the one-line mission changed materially, update the entry in ROSTER_FILE too.
5) Summarize the diff: what changed and why.

## Guardrails
- Keep the assistant focused—don’t turn it into a generalist.
- Prefer concrete checklists and rules over vague statements.
