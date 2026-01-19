# 🚀 Startup

> **Build your AI dev team** — A context isolation framework for orchestrating specialized agents scoped to specific domains of your codebase.

```
/hire dinesh a backend developer focused on API security and validation
```

---

## What is This?

Startup is a supplemental library of / commands that introduces **context-isolated AI agents** to your Cursor IDE workflow. Instead of relying on a single generalist AI that attempts to understand your entire codebase, you create purpose-built "employees" — each with a defined mission, scope, and operating rules.

Each agent becomes a **slash command** (e.g., `/dinesh`, `/api-guardian`) that knows:
- What parts of the codebase it owns
- What conventions to follow
- When to defer to other agents

Think of it as **building your own team of highly skilled professionals**, each with their own domain expertise.

---

## Why? The Context Engineering Problem

### The Generalist Trap

When working with AI coding assistants on large projects, you face a fundamental tension:

| Challenge | Symptom |
|-----------|---------|
| **Context window limits** | AI "forgets" earlier conversation, breaks patterns |
| **Convention drift** | Each response invents slightly different patterns |
| **Scope creep** | Simple tasks snowball into refactors |
| **Tribal knowledge loss** | Hard-won decisions about architecture fade |

### The Vertical Approach Problem

Most context engineering frameworks take a **vertical approach**: they focus on *what* code to write across multiple domains, without emphasizing *how* it should be written within each domain.

```
Traditional Approach          Startup
─────────────────────         ────────────────────
        ┌───┐                 ┌───┬───┬───┬───┐
        │ AI│                 │ B │ F │ T │ I │
        │   │  ← one agent    │ a │ r │ e │ n │  ← specialized
        │   │    does all     │ c │ o │ s │ f │    agents own
        │   │                 │ k │ n │ t │ r │    domains
        │   │                 │ e │ t │ s │ a │
        │   │                 │ n │   │   │   │
        │   │                 │ d │   │   │   │
        └─┬─┘                 └───┴───┴───┴───┘
          │                         │
    ┌─────┼─────┐             Each agent knows
    ▼     ▼     ▼             HOW to code in
  API   UI   Tests            its domain
```

This vertical approach works for starter projects, but quickly becomes **convoluted for larger codebases**. Without domain boundaries, the AI juggles too many concerns, loses track of conventions, and produces inconsistent code.

**Our goal: empower developers to become managers of their own dev team.**

### The Solution: Context Isolation

Rather than fighting context limits with ever-larger windows, **Startup** takes the opposite approach: **divide and specialize**.

```
┌───────────────────────────────────────┐
│            YOUR CODEBASE              │
├───────────────────┬───────────────────┤
│     /dinesh       │     /gilfoyle     │
│    API routes     │      DevOps       │
│    validation     │    deployment     │
│     security      │    monitoring     │
└───────────────────┴───────────────────┘
```

Each agent operates with **focused context**:
- Fewer files to consider → better decisions
- Explicit conventions → consistent output
- Clear boundaries → knows when to stop

---

## How It Works

### Architecture

```
.cursor/
└── commands/
    ├── _roster.md      # Registry of active agents
    ├── hire.md         # Meta-command: create new agents
    ├── fire.md         # Meta-command: remove agents
    ├── update.md       # Meta-command: modify agents
    ├── list.md         # Meta-command: show all agents
    ├── dinesh.md       # ← Hired agent created with /hire
    └── gilfoyle.md     # ← Hired agent created with /hire
```

### The Roster File (`_roster.md`)

The roster is the **source of truth** for your agent workforce. It tracks:
- Active agents and their missions
- Focus areas and touched modules
- Quick reference for team onboarding

### Agent Command Files

Each hired agent is a markdown file that defines:

```markdown
# /dinesh — Backend API Guardian

## Mission (1 sentence)
Ensure API routes are secure, validated, and follow established patterns.

## Scope
### In scope
- Route handlers in /api/*
- Request validation with Zod
- Authentication middleware
- Error response formatting

### Out of scope
- Database schema changes
- Frontend API clients
- Deployment configuration

## Interfaces (where it operates)
- src/api/**
- src/middleware/auth*
- src/validators/**

## Project Conventions Observed
- All routes use Express Router
- Validation happens in middleware layer
- Errors return { error: string, code: number }
- Auth uses JWT with refresh tokens

## Operating Rules
- [ ] Every new route has input validation
- [ ] Auth-required routes use authMiddleware
- [ ] Error responses follow standard format
- [ ] Rate limiting on public endpoints
```

---

## Usage

### Hiring an Agent

```
/hire <name> <role description>
```

**Examples:**

```
/hire dinesh a backend developer focused on API security and scalability. 
He owns Express routes, middleware, and request validation. 
He enforces Zod schemas on all inputs, implements rate limiting, 
and ensures consistent error responses across endpoints.

/hire gilfoyle a devops specialist focused on infrastructure-as-code on AWS.
He owns Terraform configs, CI/CD pipelines, and container orchestration.
He enforces least-privilege IAM policies, manages auto-scaling rules,
and maintains observability with CloudWatch and alerting.
```

The `/hire` command:
1. Scans your codebase to detect conventions
2. Generates a scoped agent definition
3. Registers it in the roster
4. Makes it available as a slash command

### Using a Hired Agent

Simply invoke the agent by name:

```
/dinesh add rate limiting to the /users endpoint

/gilfoyle set up auto-scaling for the API containers
```

The agent will:
- Stay within its defined scope
- Follow project conventions
- Defer to other agents when appropriate
- Provide structured output (what changed, why, how to verify)

### Listing Your Team

```
/list
```

Shows:
- System commands (hire, fire, update, list)
- Active agents with missions and focus areas

### Updating an Agent

```
/update <name> <changes>
```

**Example:**

```
/update dinesh add OWASP security checklist and enforce Zod on all routes
```

### Removing an Agent

```
/fire <name>
```

---

## Practical Examples

### Example 1: Building a SaaS Backend

**Hire your team:**

```
/hire dinesh a backend developer focused on API security and scalability.
He owns Express routes, middleware, and request validation.
He enforces Zod schemas on all inputs, implements rate limiting,
and ensures consistent error responses across endpoints.

/hire gilfoyle a devops specialist focused on infrastructure-as-code on AWS.
He owns Terraform configs, CI/CD pipelines, and container orchestration.
He enforces least-privilege IAM policies, manages auto-scaling rules,
and maintains observability with CloudWatch and alerting.
```

**Work with focused context:**

```
/dinesh add a PATCH /users/:id endpoint with email validation

/dinesh implement rate limiting on all public endpoints

/gilfoyle configure auto-scaling for the API containers

/gilfoyle set up CloudWatch alarms for error rate spikes
```

### Example 2: Security Hardening

**Task your specialists:**

```
/dinesh review the authentication flow for session fixation risks

/dinesh add OWASP-compliant input validation to all user endpoints

/gilfoyle audit IAM roles and enforce least-privilege access

/gilfoyle enable VPC flow logs and set up intrusion detection
```

### Example 3: Scaling for Launch

**Coordinate the team:**

```
/dinesh optimize database queries for the dashboard endpoint

/dinesh add caching layer for frequently accessed user data

/gilfoyle provision read replicas for the production database

/gilfoyle configure CDN for static assets
```

---

## Best Practices

### 1. Start Narrow, Expand Later

Begin with 2-3 focused agents. Observe where boundaries blur, then refine.

### 2. Name Agents Meaningfully

Choose names that help you remember what each agent does — whether by domain (`/api-guardian`) or personality (`/dinesh`, `/gilfoyle`).

### 3. Hire with Purpose

When hiring an agent, provide a few focused sentences covering:

| Aspect | Question | Example |
|--------|----------|---------|
| **Role** | What do they do? | "A backend developer..." |
| **Mission** | What are they in charge of? | "...focused on API endpoints and security." |
| **Context** | What tools/knowledge do they have? | "Writes TypeScript, talks to MongoDB." |
| **Boundaries** | Where do they operate? | "All code is in the `/api` folder." |

If you can't fit an agent into this structure cleanly, split it into multiple agents.

### 4. Document Observed Conventions

When hiring an agent, ensure it captures actual project patterns. This becomes living documentation.

### 5. Use Agents for Knowledge Retention

Agents persist conventions that would otherwise be forgotten between sessions. Treat them as institutional memory.

---

## Installation

1. Copy the `.cursor/commands/` folder to your project root
2. Ensure Cursor recognizes the commands folder (check Cursor settings)
3. Start hiring agents with `/hire`

---

## Philosophy

Traditional context engineering tries to **cram more into the window**. Startup takes the opposite approach: **scope down to what matters**.

This mirrors how effective human teams work:
- Specialists own domains
- Clear handoffs between areas
- Shared conventions, local expertise

By giving AI assistants the same structure, you get:
- **Consistency** — Agents follow their documented patterns
- **Focus** — Less noise, better signal
- **Scalability** — Add agents as your codebase grows
- **Knowledge persistence** — Conventions survive sessions

---

## Roadmap

- [ ] **Multi-IDE support** — Adapt to other IDEs and frameworks (VS Code, Windsurf, etc.)
- [ ] **Constitution file** — Add a top-level governance document (`_constitution.md`) with project-wide rules all agents must follow (TBD)
- [ ] **Agent dependencies** — Define relationships between agents (e.g., `/dinesh` consults `/gilfoyle` on deployment)
- [ ] **Handoff protocol** — Standardize how agents pass context when deferring to each other
- [ ] **Team templates** — Pre-built agent teams for common stacks (SaaS, mobile, data pipeline)
- [ ] **Conflict resolution** — Handle overlapping scopes gracefully when two agents could own a task

---

<p align="center">
  <i>"The best code is written by agents who know their place."</i>
</p>
