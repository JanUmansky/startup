# /fire — Remove an existing assistant command

Definitions:
- COMMANDS_DIR = the folder where command markdown files live.
- ROSTER_FILE = COMMANDS_DIR + "/_roster.md"
- Command file path = COMMANDS_DIR + "/<name>.md"

Goal:
Remove an assistant command file and unregister it from ROSTER_FILE.

## Input
User message: "/fire <name>" (e.g., "/fire bob" or "/fire api-guardian")

## Steps
1) Extract assistant name.
2) Resolve the command file:
   - Prefer exact match COMMANDS_DIR/<name>.md
   - If not found, search within ROSTER_FILE for close matches and select the best candidate.
3) Delete the assistant command file.
4) Update ROSTER_FILE:
   - Remove the assistant’s entry under “Active Assistants”.
5) Report what was removed.

## Safety
- Only delete within COMMANDS_DIR unless explicitly asked.
