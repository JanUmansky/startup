# /define — Create project-wide coding rules and conventions

You are operating in a repository that supports scoped assistants. This command creates **global rules** that apply to ALL agents across the codebase.

Definitions:
- RULES_DIR = ".cursor/rules"
- Rules are stored as `.mdc` files (markdown with frontmatter) in RULES_DIR

Goal:
Analyze the codebase and create (or update) rules that define project-wide conventions. These rules will be followed by all agents operating in this repository.

## Input (from the user)
The user message invoking /define may contain:
- **No additional input**: Analyze the codebase automatically and infer best practices
- **Specific instructions**: Combine user preferences with code analysis

Examples:
- "/define" → Full automatic analysis
- "/define prefer arrow functions and single quotes" → User preference + analysis
- "/define document all public functions with JSDoc" → Specific rule request

## Rule Categories to Consider
When analyzing the codebase, focus on these general topics:

### 1. Naming Conventions
- Variables: camelCase, snake_case, PascalCase
- Functions: naming patterns, prefixes (get, set, is, has, handle, on)
- Files: kebab-case, PascalCase, suffixes (.util, .service, .test)
- Constants: SCREAMING_SNAKE_CASE vs other
- Components/Classes: naming patterns

### 2. Folder Structure
- Directory organization patterns
- Separation of concerns (features, layers, domains)
- Where different file types belong
- Barrel files (index.ts) usage

### 3. Code Style
- Arrow functions vs function declarations
- Named exports vs default exports
- Destructuring preferences
- Ternary vs if/else for simple conditions
- Early returns vs nested conditions

### 4. Text Formatting
- Multiline strings: template literals, line breaks, concatenation
- String quotes: single, double, template
- Object/array formatting: single-line vs multi-line thresholds
- Max line length preferences

### 5. Comments & Documentation
- When to comment (complex logic, public APIs, non-obvious code)
- Comment style (inline, block, JSDoc/TSDoc)
- README requirements for folders/modules
- TODO/FIXME conventions

### 6. Error Handling
- Try/catch patterns
- Error message formatting
- Custom error classes
- Logging conventions

### 7. Imports & Dependencies
- Import ordering (builtin, external, internal, relative)
- Path aliases usage
- Barrel imports vs direct imports

### 8. Testing Conventions
- Test file naming and location
- Describe/it structure
- Mocking patterns
- Coverage expectations

## Steps

1) **Ensure RULES_DIR exists**
   - Create `.cursor/rules` folder if missing

2) **Parse user input**
   - If empty: proceed with full automatic analysis
   - If provided: note specific preferences to incorporate

3) **Scan existing rules**
   - List all `.mdc` files in RULES_DIR
   - Read each file's frontmatter (globs, alwaysApply) and content headers
   - Build a mental map of what rules already exist and their scope
   
   **Existing rule index example:**
   ```
   python.mdc       → globs: *.py           → Python naming, imports, docstrings
   conventions.mdc  → alwaysApply: true     → General naming, style, formatting
   api-routes.mdc   → globs: src/api/**     → REST conventions, response format
   ```

4) **Analyze the codebase**
   - Scan for existing patterns across file types
   - Look for config files: .eslintrc, .prettierrc, tsconfig.json, package.json, etc.
   - Identify dominant conventions already in use
   - Note any inconsistencies that need resolution
   
   Analysis targets:
   - Source files (js, ts, jsx, tsx, py, go, etc.)
   - Config files
   - Existing documentation
   - Test files
   - Package managers and dependencies

5) **Determine rule set**
   - Prioritize user-specified preferences
   - Infer conventions from existing code patterns
   - Fill gaps with sensible defaults based on ecosystem best practices
   - Resolve any conflicts (user input wins)

6) **Decide: UPDATE existing rule or CREATE new rule**
   
   This is a critical decision. When the user provides specific instructions:
   
   ### UPDATE an existing rule file when:
   - User's request relates to a language/folder that already has a rule file
     - Example: User says "Python scripts should use type hints" → Update `python.mdc`
   - User's request fits within an existing category
     - Example: User says "use single quotes" → Update `conventions.mdc` (if it exists and covers style)
   - The glob pattern would be the same or overlapping
     - Example: User says "all .py files should..." and `python.mdc` already has `globs: *.py`
   
   ### CREATE a new rule file when:
   - No existing rule covers the topic/language/folder
     - Example: User says "Go files should..." but no `go.mdc` exists → Create `go.mdc`
   - The request is for a new, distinct scope
     - Example: User says "database migrations should..." → Create `migrations.mdc`
   - Mixing it with existing rules would create confusion
   
   ### Decision flowchart:
   ```
   User request about X
          │
          ▼
   Does a rule file exist that covers X?
          │
     ┌────┴────┐
     │         │
    YES        NO
     │         │
     ▼         ▼
   UPDATE    CREATE
   existing   new
   rule       rule
   ```
   
   ### When updating:
   - Read the existing file completely
   - Add new rules to the appropriate section
   - If a new section is needed, add it following the existing structure
   - Preserve all existing rules unless user explicitly wants to change them
   - Keep the frontmatter intact (globs, alwaysApply)

7) **Create/update rule files**
   Structure rules into logical `.mdc` files:
   
   - `RULES_DIR/conventions.mdc` — Core coding conventions (naming, style, formatting)
   - `RULES_DIR/structure.mdc` — Folder organization and file placement
   - `RULES_DIR/documentation.mdc` — Comments, docs, README requirements
   
   Or create a single comprehensive file:
   - `RULES_DIR/project-rules.mdc` — All rules in one place
   
   Choose based on codebase size and complexity.
   
   **File-specific rules** (for language/folder patterns):
   - `RULES_DIR/python.mdc` — Python-specific conventions (globs: *.py)
   - `RULES_DIR/typescript.mdc` — TypeScript conventions (globs: *.ts, *.tsx)
   - `RULES_DIR/api-routes.mdc` — API folder rules (globs: src/api/**)
   - `RULES_DIR/tests.mdc` — Testing conventions (globs: **/*.test.*, **/*.spec.*)

8) **Report what was created or updated**
   - Specify whether each file was CREATED or UPDATED
   - List the specific rules added/modified
   - Summarize key conventions established
   - Note any areas that need manual review

## Rule File Format

**CRITICAL**: Every `.mdc` rule file MUST begin with a YAML frontmatter block containing at least ONE of:
- `globs` — Pattern matching for specific files/folders
- `alwaysApply: true` — For global rules that apply everywhere

### Frontmatter Options

| Field | Purpose | Examples |
|-------|---------|---------|
| `globs` | Apply to files matching pattern(s), comma separated, no quotes | `*.py`, `src/api/**`, `*.test.ts`, `**/*.js,**/*.jsx` |
| `alwaysApply` | Apply globally to all files | `true` or `false` |

**Rules:**
- At least ONE of `globs` or `alwaysApply` MUST be present
- Both can be present if needed
- `globs` can be a single pattern or comma-separated list
- Frontmatter block uses `---` delimiters

### Template: Global Rule (alwaysApply)

```markdown
---
alwaysApply: true
---

# [Category] Rules

> These rules apply to ALL agents operating in this repository.

## [Subcategory]

### Rule: [Short name]
**Convention:** [The rule]
**Rationale:** [Why this matters]
**Examples:**
// ✅ Do this
// ❌ Not this
```

### Template: File-Specific Rule (globs)

```markdown
---
globs: *.py
---

# Python Coding Standards

> These rules apply when working with Python files.

## Naming Conventions
- Use snake_case for functions and variables
- Use PascalCase for classes
...
```

### Template: Folder-Specific Rule (globs)

```markdown
---
globs: src/api/**
---

# API Route Conventions

> These rules apply to all files in the API folder.

## Route Structure
- Each route file exports a single router
- Use RESTful naming conventions
...
```

### Template: Combined (globs + alwaysApply)

```markdown
---
globs: *.ts, *.tsx
alwaysApply: false
---

# TypeScript Standards

> These rules apply to TypeScript files only.
```

## Output Format

Structure your response clearly with emojis, tables, and lists:

---

### 📋 Summary

> One sentence describing what was done.

Example: *Analyzed codebase and established Python coding standards based on existing patterns.*

---

### 📁 Files

| Status | File | Scope |
|--------|------|-------|
| ✨ Created | `python.mdc` | `globs: *.py` |
| 📝 Updated | `conventions.mdc` | `alwaysApply: true` |

**Location:** `.cursor/rules/`

---

### 📜 Rules Established

List the key rules with emojis:

| Category | Rule |
|----------|------|
| 🐍 Python | Use snake_case for functions |
| 🐍 Python | Docstrings required for public functions |
| 🎨 Style | Single quotes for strings |
| 📁 Structure | Group imports: stdlib → external → internal |

---

### 🔍 Analysis Notes

- ✅ Detected: *What patterns were found*
- ⚠️ Inconsistency: *Any conflicts noted*
- 💡 Suggestion: *Recommendations for manual review*

---

### ✅ Next Steps

> These rules are now active for all agents. Use `/list` to see your team, or `/hire` to add specialists.

## Important Behaviors

- **Update over create**: Always check existing rules first — extend them rather than creating duplicates
- **Be descriptive, not prescriptive**: Rules should explain the "why" not just the "what"
- **Use examples**: Show good and bad patterns
- **Stay practical**: Focus on rules that actually impact code quality
- **Detect before dictating**: Existing patterns in the codebase take precedence unless user specifies otherwise
- **Keep it DRY**: Reference external configs (eslint, prettier) rather than duplicating them
- **Make rules scannable**: Use clear headers and bullet points
- **Preserve existing content**: When updating, never remove existing rules unless explicitly asked

## Edge Cases

- **Empty codebase**: Create sensible defaults based on detected tech stack or ask user for preferences
- **Mixed conventions**: Note the inconsistency and ask user which to standardize on
- **Conflicting configs**: Prefer explicit user input > .cursor/rules > project configs > ecosystem defaults
- **Overlapping scopes**: If user request could fit multiple existing rules, choose the most specific one
- **Ambiguous topic**: If unclear whether to update or create, prefer updating an existing related rule

### Update vs Create Examples

| User says | Existing rules | Action |
|-----------|----------------|--------|
| "Python functions need docstrings" | `python.mdc` exists | **Update** `python.mdc` |
| "Use snake_case in Python" | `conventions.mdc` exists (general) | **Create** `python.mdc` (language-specific) |
| "All files need header comments" | `conventions.mdc` exists | **Update** `conventions.mdc` |
| "Go code should use gofmt" | No Go rules exist | **Create** `go.mdc` |
| "API routes return JSON" | `api-routes.mdc` exists | **Update** `api-routes.mdc` |
| "Tests use describe/it" | `tests.mdc` exists | **Update** `tests.mdc` |
| "Tests use describe/it" | No test rules exist | **Create** `tests.mdc` |

Now implement: analyze the codebase and create the appropriate rule files.
