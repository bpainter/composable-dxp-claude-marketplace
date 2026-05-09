---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[technical-writer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Documentation Frameworks & Types

Deep reference on the doc types, templates, and frameworks that underpin effective technical documentation systems.

---

## The Diátaxis Framework

**Diátaxis** (by Daniele Procida) organizes documentation into four pillars. Each serves a distinct user need:

### Tutorials (Learning-Oriented)
**Purpose**: Get someone from zero knowledge to first successful working example.

**Audience**: Complete beginners; people with high motivation but zero context.

**Structure**:
1. Clear goal upfront ("By the end, you'll have deployed your first microservice")
2. Minimal explanation—assume nothing, explain as you go
3. Copy-paste ready code
4. Short steps (one concept per step)
5. Success criteria: "You should see X in your terminal"

**Examples**: "Your First API Call", "Set Up a Local Dev Environment", "Deploy Your First Lambda Function"

**Anti-patterns**: Optimizing for brevity, assuming prior knowledge, diving into alternatives, mixing with reference material.

### How-to Guides (Problem-Oriented)
**Purpose**: Solve a specific, named problem. User knows what they want; they're looking for the path.

**Audience**: People with context; they have a goal and need the shortest path to it.

**Structure**:
1. Goal statement ("How to roll back a deployment without downtime")
2. Prerequisite check ("You need X, Y, Z in place")
3. Numbered steps (action-oriented, not explanatory)
4. If/then branches for common variations
5. Links to deeper explanation if the "why" matters

**Examples**: "How to Add a New API Endpoint", "How to Rotate Secrets Without Downtime", "How to Set Up Database Migrations"

**Anti-patterns**: Explaining every concept, multi-purpose solutions, mixing learning and doing.

### Reference (Information-Oriented)
**Purpose**: Complete, accurate spec. The source of truth.

**Audience**: People who already understand the domain; they need to look something up.

**Structure**:
1. Complete list/spec: all parameters, all options, all error codes
2. Organized for lookup: alphabetical, by type, by use case
3. Code examples for every entry (when applicable)
4. Cross-references to how-to guides ("See also: How to Set Request Timeouts")

**Examples**: API reference, CLI flag reference, config schema reference, all error codes and what to do about them.

**Anti-patterns**: Explanatory prose, hidden parameters, incomplete examples.

### Explanation (Understanding-Oriented)
**Purpose**: Understand why something works the way it does; trade-offs and context.

**Audience**: People going deeper; they want to make informed decisions.

**Structure**:
1. Problem/context: why this thing exists
2. Design choices and tradeoffs
3. Alternatives and why they weren't chosen
4. Common gotchas and why they happen
5. Links to reference for detail, how-to for execution

**Examples**: "Why We Use Event Sourcing", "Understanding Rate Limiting", "How Caching Layers Trade Consistency for Speed"

**Anti-patterns**: Overwhelming the reader, mixing with reference data, assuming deep background.

---

## Architecture Decision Records (ADRs)

ADRs document significant technical decisions and their rationale. They live in Git, ship with code, and prevent repeated mistakes.

### ADR Template (Nygard Format)

```
# ADR [number]: [short title]

## Status
Accepted | Proposed | Superseded | Deprecated

## Context
What is the issue that we're seeing that motivates this decision or change?

## Decision
What is the change that we're proposing or have decided to do?

## Consequences
What becomes easier or more difficult to do because of this change?

## Rationale
Why did we make this choice? What were the alternatives?

## Related Decisions
Links to other ADRs that informed or were informed by this decision.
```

### Lightweight ADR Template (MADR)
For teams that want faster, shorter ADRs:

```
# [Short Title]

## Question or Issue
[1-2 sentence problem statement]

## Considered Options
- Option A: [approach and rationale]
- Option B: [approach and rationale]

## Decision Outcome
Chosen: Option A

### Positive Consequences
- [consequence]

### Negative Consequences
- [consequence]
```

### ADR Best Practices
- **One decision per ADR**: Don't bundle multiple decisions.
- **Write in past tense once decided**: "We decided to..." not "We should decide to..."
- **Include alternatives**: Show what you *didn't* choose and why.
- **Link to code**: Reference pull requests, commits, or architecture docs that implement the decision.
- **Index them**: Build an ADRLOG or index file that lists all ADRs by status.
- **Review like code**: ADRs should go through the same review process as commits.
- **Supersede, don't delete**: If a decision changes, mark the old one as "Superseded by ADR #N" instead of deleting it.

### ADR Decision Log (Index)
Keep a simple index of all ADRs:

```
# Architectural Decision Log

## Accepted
- ADR 1: Use PostgreSQL for primary data store
- ADR 2: Implement event-driven architecture for real-time notifications
- ADR 4: Use OpenAPI 3.x for API specification

## Proposed
- ADR 6: Migrate from REST to GraphQL

## Superseded
- ADR 3: Use MongoDB (superseded by ADR 1)
- ADR 5: Use custom pub/sub (superseded by ADR 2)
```

---

## RFCs & Design Docs

RFCs (Request for Comments) are proposals for significant features, architectural changes, or process decisions. They invite stakeholder input before implementation.

### RFC Template

```
# RFC: [Short Title]

## Summary
[1 paragraph: what are we doing and why?]

## Motivation
Why do we need this? What problem does it solve? What's the impact if we don't do this?

## Detailed Design
- How will this work end-to-end?
- Diagram or pseudocode if helpful
- API/schema changes
- Database changes
- Deployment considerations

## Alternatives
- Option A: [why we didn't choose this]
- Option B: [why we didn't choose this]

## Risks & Mitigations
- Risk: [consequence if something goes wrong] → Mitigation: [how we'll prevent/handle it]

## Testing & Rollout
- Testing strategy
- Rollout plan (feature flags, phased rollout, etc.)
- Rollback plan

## Open Questions
[Questions for reviewers; what we're uncertain about]

## Approval
- [ ] Tech lead review
- [ ] Security review
- [ ] Product approval
```

### RFC Best Practices
- **Async-first**: Post the RFC; give reviewers 48-72 hours to read and comment before discussing.
- **Clear decision maker**: Who has final call? RFC doesn't replace decision authority.
- **Narrow scope**: Large changes should split into multiple RFCs (data model, API design, implementation).
- **Reference decisions**: Link to relevant ADRs, design docs, and past RFCs.
- **Archive after approval**: Move approved RFCs to a design docs folder; they become reference material.

---

## Runbooks & Playbooks

Runbooks document operational procedures: incident response, deployments, maintenance tasks. They're read under pressure and must be crystal clear.

### Runbook Template

```
# [Runbook Title]

## Purpose
One sentence: what problem does this solve?

## Scope & Severity
- Applies to: [service, system, or scenario]
- Severity: Critical | High | Medium | Low
- Estimated time: X minutes

## Prerequisites
- [ ] You have access to [tools/systems]
- [ ] [Service/system] is in [state]
- [ ] You understand [concept]

## Steps

### Phase 1: Diagnosis
1. Check [X] with command: `[command]`
   - Expected: [output]
   - If different: [proceed to escalation]

2. Look for [error pattern] in logs with: `[command]`

### Phase 2: Immediate Response
1. [Action] by running: `[command]`
2. Wait for [condition]; verify with: `[command]`

### Phase 3: Verification
1. Run: `[command]`
2. Expected output: [output]
3. If successful: proceed to "Success & Notification"
4. If failed: proceed to "Rollback"

## Rollback
If Phase 2 fails or causes worse problems:
1. Run: `[command]`
2. Verify with: `[command]`
3. Expected state: [state]

## Escalation
If you see [symptom], immediately:
1. Page [on-call person/team]
2. Run diagnostics: `[command]`
3. Provide logs to escalated team

## Success & Notification
1. Post in #incidents: "[Service] issue resolved. Root cause: [X]. Fix: [Y]."
2. Create incident ticket with:
   - Timeline
   - What happened
   - Why it happened
   - Permanent fix or prevention (if applicable)

## Common Issues & Solutions
| Symptom | Likely Cause | Solution |
|---------|-------------|----------|
| [Error] | [Cause] | [Fix] |

## Related Docs
- [Link to architectural decision]
- [Link to service design doc]
- [Link to deployment procedure]
```

### Runbook Best Practices
- **Assume panic**: Write for someone under stress with partial sleep.
- **Copy-paste ready**: Every command should be tested and ready to run.
- **Explicit success criteria**: How do you know you succeeded?
- **Rollback path**: Every mitigation step should have a undo plan.
- **Version with code**: Runbooks live in Git, reviewed like code, versioned with releases.
- **Test them**: Chaos engineering + runbook drills.
- **Short & scannable**: Aim for <2000 words; use tables and lists.

---

## API Documentation

API docs are your contract with users (internal or external). They must be complete, accurate, and example-heavy.

### API Doc Structure

```
# [Service] API

## Overview
[2-3 sentences: what is this API for?]
[Link to tutorials/how-to guides for common use cases]

## Authentication
- Mechanism: [API key | OAuth 2.0 | JWT | etc.]
- How to obtain: [link]
- Example:
  ```
  curl -H "Authorization: Bearer YOUR_API_KEY" https://api.example.com/...
  ```

## Rate Limiting
- Rate limit: X requests per Y minutes
- Headers returned: [header names]
- Behavior when limit exceeded: [what happens]

## Errors
| Status | Code | Meaning | Recovery |
|--------|------|---------|----------|
| 400 | INVALID_REQUEST | [description] | Check that [X] is valid |
| 401 | UNAUTHORIZED | Your credentials are invalid | Get new API key from [link] |
| 429 | RATE_LIMITED | Too many requests | Wait [X] seconds before retrying |

## Endpoints

### Create Thing
`POST /v1/things`

**Request:**
```json
{
  "name": "string (required)",
  "config": {
    "timeout": "integer (optional, default: 30)"
  }
}
```

**Response (201 Created):**
```json
{
  "id": "string",
  "name": "string",
  "created_at": "RFC3339 timestamp",
  "status": "active"
}
```

**Example:**
```bash
curl -X POST https://api.example.com/v1/things \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "my-thing", "config": {"timeout": 60}}'
```

**Error Cases:**
- 400: name is missing or empty
- 409: name already exists

---

### Get Thing
`GET /v1/things/{id}`

**Response (200 OK):**
```json
{
  "id": "string",
  "name": "string",
  ...
}
```

**Example:**
```bash
curl https://api.example.com/v1/things/123 \
  -H "Authorization: Bearer YOUR_API_KEY"
```
```

### API Doc Best Practices
- **OpenAPI/Swagger spec first**: Write the spec; use tools to generate the portal.
- **Copy-paste curl/SDK examples**: Every endpoint should have working examples.
- **Document error cases**: For every error code, explain the cause and recovery.
- **Versioning strategy**: How do you handle breaking changes? (API versioning, deprecation periods, parallel versions?)
- **SDKs in multiple languages**: If targeting external devs, provide Python, JavaScript, Go examples.
- **Changelog for breaking changes**: Document deprecations with timeline and migration path.
- **Test examples**: Broken examples lose trust; CI should validate all example code.

---

## README Templates

### Minimal README (for libraries/tools)

```markdown
# Project Name

[1 sentence: what is this?]

## Features
- Feature A
- Feature B

## Quick Start

### Installation
```bash
npm install project-name
```

### Basic Usage
```python
from project_name import Thing

t = Thing("config")
t.do_something()
```

See [documentation](./docs/README.md) for more.

## API Reference
- [Full API docs](./docs/API.md)

## Contributing
- [Development setup](./docs/DEVELOPMENT.md)
- [Code style](./docs/STYLE_GUIDE.md)

## License
MIT
```

### Comprehensive README (for services/platforms)

```markdown
# Project Name

[Elevator pitch: who uses this, and why?]

## Table of Contents
- [Features](#features)
- [Getting Started](#getting-started)
- [Architecture](#architecture)
- [API Reference](#api-reference)
- [Contributing](#contributing)

## Features

## Getting Started

### 1. Prerequisites
- Node.js 18+
- Docker
- AWS credentials

### 2. Installation
[Step-by-step; should take 5 minutes]

### 3. Your First Deploy
[Success = new hire can deploy in 10 minutes]

## Architecture
[Diagram + short explanation of major components]

## Documentation
- [User guide](./docs/guide.md)
- [API reference](./docs/api.md)
- [Configuration](./docs/config.md)
- [Troubleshooting](./docs/troubleshooting.md)

## Contributing
- [Development setup](./docs/DEVELOPMENT.md)
- [Running tests](./docs/TESTING.md)
- [Code review process](./CONTRIBUTING.md)

## Support
- [FAQ](./docs/FAQ.md)
- [Issues](./issues)
- [Discussions](./discussions)

## License
MIT
```

### README Anti-Patterns
- **Too long**: If your README is >3000 words, link to docs rather than inlining.
- **Assumes too much**: "Install dependencies and run the build" assumes the reader has run builds before.
- **No success criteria**: Reader doesn't know when they've succeeded.
- **Outdated examples**: Make README auto-tested to prevent drift.

---

## Docs-as-Code Toolchains

### Popular Generators
- **MkDocs**: Python-based; great for internal docs, API docs; easy to host on GitHub Pages
- **Docusaurus**: React-based; versioning, multi-language support; polished out of the box
- **Mintlify**: AI-powered API docs; beautiful UI; great for external-facing APIs
- **Hugo**: Go-based; blazing fast; less opinionated than MkDocs
- **Nextra**: Next.js-based; modern, fast, great for product docs

### CI/CD Checks
Automate quality gates:

```yaml
# .github/workflows/docs.yml
name: Docs Quality

on: [pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check for broken links
        run: npm run docs:lint-links
      - name: Check markdown
        run: npm run docs:lint-md
      - name: Verify code examples run
        run: npm run docs:test-examples
      - name: Build docs
        run: npm run docs:build
```

### Versioning Strategy
- **Git branches**: Docs for main branch (latest development) + release branches for stable versions
- **MkDocs versioning**: Automatic version selector; users can pick docs for their installed version
- **Deprecation banner**: Old versions show "This is an old version; see latest" banner
- **Breaking change log**: Document deprecations with timeline and migration guide

---

## Information Architecture for Docs

Structure your docs site to match user mental models:

### Common IA Patterns

**By Audience**:
```
/docs
├── /getting-started        # Zero to first success
├── /guides                 # Step-by-step how-tos
├── /reference              # API specs, config, CLI
├── /architecture           # Deep dives, design docs
├── /faq
└── /troubleshooting
```

**By Feature**:
```
/docs
├── /authentication
├── /real-time
├── /webhooks
├── /cli
└── /api
```

**Hybrid** (larger orgs):
```
/docs
├── /for-developers         # Dev-focused
├── /for-operators          # Operations, runbooks
├── /for-product-managers   # Business logic, metrics
└── /reference              # Shared (API, config)
```

### Information Scent Checklist
- Section headings answer "will I find what I need if I click?"
- Links have descriptive text ("See how to set up authentication" not "click here")
- Breadcrumbs help orient where you are
- Search results show context (where the match appears, not just title)
- Table of contents on every page makes structure visible

---

## Style & Tone for Technical Docs

### Key Principles
- **Active voice**: "You create a service" not "A service is created"
- **Second person**: Address the reader directly ("You'll need X")
- **Concrete over abstract**: "Run `npm install` in the project root" vs "Install dependencies"
- **Example-first**: Show the code, then explain
- **Progressive disclosure**: Show simple path first, link to complexity
- **Inclusive language**: Avoid gendered pronouns, assume nothing about reader background

### Voice & Tone Matrix

| Context | Tone | Example |
|---------|------|---------|
| Tutorial (first time) | Encouraging, step-by-step | "Let's deploy your first function. You've got this." |
| How-to (known goal) | Direct, action-oriented | "Run this command. You'll see X. If not, check Y." |
| Reference (lookup) | Precise, complete | "timeout (integer): Request timeout in milliseconds. Default: 30000." |
| Explanation (learning) | Patient, shows reasoning | "We chose PostgreSQL because... The tradeoff is..." |
| Error message (failure) | Helpful, actionable | "404: Resource not found. Check that the ID is correct. See [debugging guide]." |
