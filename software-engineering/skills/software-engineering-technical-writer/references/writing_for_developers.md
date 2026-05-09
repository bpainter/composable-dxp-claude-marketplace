---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[technical-writer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Writing for Developers: Craft & Process

Deep reference on the writing craft, developer psychology, and operational practices that make technical documentation effective.

---

## Clarity Principles for Technical Audiences

Developers optimize for efficiency. They skim aggressively. Bad docs waste their time and they resent you for it.

### Principle 1: One Idea Per Sentence

**Bad:**
"When you call the API, if you pass an invalid token and the service is under heavy load, it might time out or return a 500 error."

**Good:**
"Invalid tokens return 401. High load may cause timeouts (see rate limiting)."

**Why**: Developer reading under pressure doesn't parse complex conditionals. Short sentences = faster comprehension.

### Principle 2: Active Voice, Concrete Subjects

**Bad:**
"It is important that the value be validated before sending."

**Good:**
"Validate the value before calling send()."

**Why**: "It" is vague. "Important" is hedging. Developers want action and agency.

### Principle 3: Show, Then Tell

**Bad:**
"The timeout parameter controls how long the client will wait before abandoning a request. It's expressed in milliseconds."

**Good:**
```javascript
client.timeout = 5000; // Wait up to 5 seconds
```
Then: "timeout (integer): milliseconds before request is abandoned. Default: 30000."

**Why**: Code is precise. Prose explanation should clarify, not lead.

### Principle 4: Avoid Hedging

**Bad:**
"It might be possible that you could consider using environment variables, which may provide some level of security."

**Good:**
"Use environment variables for secrets. Never hardcode them."

**Why**: Developers want confident guidance. Hedging reads as uncertain/untrustworthy.

### Principle 5: Use Parallel Structure

**Bad:**
"Create the config file, then the database should be initialized, and after that you'll want to start the server."

**Good:**
"1. Create the config file. 2. Initialize the database. 3. Start the server."

**Why**: Parallel structure is faster to parse and remember.

---

## Code Sample Best Practices

Code examples are your most trusted content. Broken or outdated examples destroy credibility instantly.

### Sample Structure

Every code example should have:

1. **Context line** (comment or prose): "Create a new user"
   ```javascript
   // Create a new user
   const user = await client.users.create({
     name: "Alice",
     email: "alice@example.com"
   });
   ```

2. **Minimal but complete**: Runnable as-is (or clearly shows what's missing)
   ```python
   import requests

   response = requests.post(
       "https://api.example.com/v1/users",
       headers={"Authorization": f"Bearer {API_KEY}"},
       json={"name": "Alice", "email": "alice@example.com"}
   )
   print(response.json())
   ```
   *Assume* the reader has `API_KEY` set.

3. **Expected output** (comment showing what to expect):
   ```python
   # Output:
   # {'id': '123', 'name': 'Alice', 'email': 'alice@example.com', 'created_at': '2026-04-03T...'}
   ```

### Sample Anti-Patterns

- **Pseudocode**: "something like `user = create(name: 'Alice')`" — don't do this. Show real code.
- **Abbreviated examples**: Omitting error handling or setup that the reader actually needs.
- **Language inconsistency**: Show cURL + Python + JavaScript, not cherry-picked samples.
- **Outdated**: Library version changed, API renamed, example is now broken.
- **Environment-specific**: Hardcoded paths, URLs, or credentials that don't work on reader's machine.

### Multi-Language Examples

For public APIs, provide curl + at least 2 SDKs:

```markdown
### JavaScript
```javascript
const client = require('example-sdk');
client.setApiKey(process.env.API_KEY);

client.users.create({name: 'Alice'});
```

### Python
```python
import example_sdk
client = example_sdk.Client(api_key=os.environ.get('API_KEY'))
client.users.create(name='Alice')
```

### cURL
```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice"}'
```
```

### Testing Examples

Your CI/CD pipeline should verify code examples work:

```bash
# Extract all JavaScript examples from docs
# Run them in a test environment
# Verify they produce the expected output
# If broken: block the PR
```

This prevents the slow entropy of docs diverging from reality.

---

## Progressive Disclosure in Documentation

Developers have different depths of need. Don't force everyone through the shallow-to-deep path; let them jump to their depth.

### Depth Levels

**Level 0: Headline**
"Deploy without downtime"

**Level 1: One-sentence summary**
"Use blue-green deployment: run the new version alongside the old, then switch traffic."

**Level 2: Tutorial (steps)**
1. Spin up new environment
2. Run migrations
3. Switch traffic
4. Destroy old environment
[Link: "Why blue-green? See the tradeoffs."]

**Level 3: Explanation (why & tradeoffs)**
"Blue-green means you're running 2x infrastructure temporarily. Alternatives: canary (gradual rollout, lower cost but longer risk window), rolling (each instance updates one at a time, lower cost but more complex)."

**Level 4: Deep dive (architecture, edge cases)**
"DNS propagation delay can mean some users hit old version after switch. Mitigate with connection draining and health checks. See ADR 7 for why we chose this."

### Design Pattern

```markdown
# Deploy Without Downtime

## TL;DR
Use blue-green: spin new version, switch traffic, destroy old.

## How (5 min)
[Copy-paste ready steps]

## Why Blue-Green?
[Tradeoffs vs. alternatives]

## Advanced: Handling Edge Cases
[Stale connections, DNS, etc.]

See also: [Related RFC], [Deployment runbook]
```

### Information Scent in Progressive Disclosure
- Headline/TL;DR must answer "is this for me?"
- Each jump to deeper level must be a *choice*, not forced reading
- Links between levels should be labeled: "Why this approach?" not "More info"
- Avoid hiding important info in Level 3+; warnings and gotchas belong at Level 1

---

## Versioned Documentation Strategy

Shipping updates means docs go stale. Versioning prevents this.

### Versioning Approaches

**Approach 1: Floating (Recommended for Internal Docs)**
- Single `/docs` folder; always reflects main branch
- Use feature flags in docs: "In v2.1+ ..." or "Deprecated in v3.0"
- In changelog: document breaking changes with migration guide
- **Pros**: Simpler, less duplication
- **Cons**: Harder for users on older versions

**Approach 2: Branched (Recommended for External/Public APIs)**
- `/docs/v1`, `/docs/v2`, `/docs/v3`
- User selects version from dropdown
- Main branch docs = bleeding edge; release branches = stable
- **Pros**: Users can read docs for their installed version
- **Cons**: More work; must keep multiple versions in sync for shared concepts

**Approach 3: Hybrid**
- Common sections (API reference) are shared; generated for each version
- Version-specific sections (tutorials, examples) live in branch
- **Pros**: Reduces duplication; user sees their version
- **Cons**: Most complex to set up

### Breaking Change Communication

In changelog + docs:

```markdown
## v2.0 (Breaking Changes)

### Removed: `POST /users` requires email
- **Old**: POST /users {name: 'Alice'} — email was optional
- **New**: email is required
- **Migration**: Update all calls to include email field
- **Example**:
  ```diff
  - POST /users {name: 'Alice'}
  + POST /users {name: 'Alice', email: 'alice@example.com'}
  ```

### Renamed: Authentication header
- **Old**: X-API-Key
- **New**: Authorization: Bearer
- **Migration guide**: [link]

### Deprecated: Webhooks V1
- **Timeline**: Removed 2026-06-30
- **Replacement**: [Webhooks V2 docs]
```

### Version Deprecation Banner
Old versions should display a warning:

```
⚠️ You're reading docs for v1.0 (deprecated).
Current version: v3.0 [View latest docs]
See migration guide: v1.0 → v3.0
```

---

## Content Reuse & Single Sourcing

Don't copy-paste doc content; it drifts and causes maintenance hell.

### Pattern 1: Shared Concept Includes
Define once, reference everywhere.

```markdown
# docs/concepts/authentication.md

## Authentication Methods
- API Key: [...]
- OAuth 2.0: [...]
- JWT: [...]

[Usage examples for each]
```

Then in other docs:
```markdown
# Getting Started

First, authenticate. See [Authentication Methods](/concepts/authentication.md#api-key).
```

**Not**: Copy the authentication section into every doc.

### Pattern 2: Code Examples as External Files

Store code examples in Git, not in Markdown:

```
examples/
├── create_user.js
├── create_user.py
├── get_user.sh
└── delete_user.go
```

In docs, include them:
~~~markdown
```javascript
{@include examples/create_user.js}
```
~~~

**Benefits**: Examples are tested as part of CI; you can't forget to update docs when code changes.

### Pattern 3: Generated Documentation
Some docs should be generated from code:

- API reference: Generate from OpenAPI spec
- CLI reference: Generate from `--help`
- Config schema: Generate from JSON schema
- Changelog: Generate from git tags + commit messages

**Tool pattern**:
```bash
# In CI:
npm run docs:generate-api  # Reads src/openapi.json, outputs docs/api.md
npm run docs:generate-cli  # Reads src/cli.ts, outputs docs/cli.md
```

**Benefit**: Docs are always in sync with code.

---

## Accessibility in Documentation

Technical docs are read by people on small screens, with ADHD, colorblind, using screen readers, under time pressure.

### Best Practices

**Headings**
- Use semantic heading levels (H1 → H2 → H3, not H1 → H3)
- Every page should have one H1
- Headings should be descriptive and scannable

**Color & Contrast**
- Don't rely on color alone to convey meaning (red = error, green = success)
- Use text + icons: "✓ Success" or "✗ Error"
- Contrast ratio 4.5:1 for normal text, 3:1 for large text (WCAG AA)

**Code Blocks**
- Syntax highlighting should have sufficient contrast
- Don't use red/green exclusively for diff blocks
- Provide plain text alternative for complex diagrams

**Links**
- Link text should be descriptive: "See ADR 3" not "click here"
- Visited vs. unvisited should be visually distinct
- External links should be marked: "Learn more about OAuth [external]"

**Tables**
- Use `<th>` headers; don't rely on visual styling
- Simple tables; split complex tables into smaller ones
- Provide row/column headers for data

**Alt Text for Diagrams**
- Don't just describe the image: "A flowchart"
- Describe what someone should learn: "Deployment flow: code → tests → staging → production"

**Skip Links**
- In long docs, provide "Skip to main content" or "Jump to API reference"

**Dark Mode**
- Test docs in dark mode; light backgrounds with dark text fail in dark mode
- Use CSS custom properties for colors, not hardcoded hex

---

## Documentation Review Process

Docs quality depends on review discipline. Make it part of your development workflow.

### Review Checklist

**Content Review** (does it answer the question?)
- [ ] Is the goal clear upfront?
- [ ] Is there a TL;DR?
- [ ] Are there copy-paste ready examples?
- [ ] Are success criteria visible?
- [ ] Are gotchas/warnings highlighted?
- [ ] Are related docs linked?

**Technical Accuracy** (is it correct?)
- [ ] Do code examples run without modification?
- [ ] Is the info current with latest release?
- [ ] Are error messages/outputs up to date?
- [ ] Are deprecated features marked?

**Clarity** (can a new person understand this?)
- [ ] Remove hedging ("might," "possibly," "could")
- [ ] Active voice where possible
- [ ] Short sentences and paragraphs
- [ ] Acronyms defined on first use
- [ ] Jargon explained

**Completeness** (what's missing?)
- [ ] All required parameters/fields documented
- [ ] All error cases covered
- [ ] Alternatives explained
- [ ] Escalation path clear

### Automation in Review

```yaml
# .github/workflows/docs-review.yml
- name: Lint markdown
  run: npm run docs:lint
  # Checks: heading levels, link validity, image alt text

- name: Test code examples
  run: npm run docs:test-examples
  # Checks: all code blocks run successfully

- name: Check for deprecated content
  run: npm run docs:check-deprecations
  # Checks: ADRs marked "Superseded", version mentions up to date

- name: Accessibility audit
  run: npm run docs:audit-a11y
  # Checks: heading structure, color contrast, link text
```

---

## Measuring Documentation Effectiveness

Docs improve when you measure them. Track these signals:

### Adoption Metrics
- **Time to first success**: How long for new user to complete first tutorial?
- **Tutorial completion rate**: What % start and finish getting started?
- **Jump to reference**: How many skip tutorials and go straight to API docs? (Segment your audience.)

### Engagement Metrics
- **Docs traffic**: How many page views? Which pages?
- **Scroll depth**: Do people read past the fold or skim and leave?
- **Time on page**: <30 sec = skimmed; >2 min = careful read
- **Search patterns**: What queries are people running? (Indicates missing content.)

### Support Metrics
- **Support ticket reduction**: Good docs reduce "how do I X?" tickets.
- **Docs quality score**: Survey users post-support interaction: "Did you find the docs helpful?"
- **Ticket correlation**: Tickets without corresponding docs → prioritize that doc.

### Velocity Metrics
- **Onboarding time**: Time from hire to "productive" (shipping code). Track trend.
- **Time to resolution**: How long to find answer in docs vs. asking on Slack?
- **Documentation churn**: How often do docs need updates? High churn = outdated + low confidence.

### How to Instrument

```javascript
// In docs site:
// Track which docs page → user then filed support ticket
// Track: docs page → user then shipped code

analytics.track('docs_page_viewed', {
  page: '/api-reference',
  section: 'authentication',
  depth_scrolled: '75%',
  time_on_page: 180,
  user_id: 'user-123'
});

// Later, if user files ticket:
analytics.track('support_ticket_filed', {
  user_id: 'user-123',
  category: 'authentication',
  last_docs_page: '/api-reference'
  // Insight: "users who read /authentication but didn't reach section X file tickets"
});
```

---

## Style Guide Essentials

Your docs should sound like one voice, not 10. Style guides enforce consistency without killing personality.

### Template Style Guide

```markdown
# [Your Project] Documentation Style Guide

## Voice & Tone
- **Direct**: "Run this command." Not "You might want to consider running..."
- **Second person**: Address the reader as "you"
- **Active voice**: Prefer "you create a user" over "a user is created"
- **Confident**: Don't hedge unless there's actual uncertainty
- **Inclusive**: Use they/them pronouns, avoid gendered language

## Structure
- Every page: Title, goal/TL;DR, steps, gotchas, next steps
- Every section: Heading that answers "what will I learn?"
- Every code example: Context + code + expected output

## Writing Rules
- Spell out acronyms on first use: "CI/CD (Continuous Integration/Continuous Deployment)"
- Numbers: 0-9 as words, 10+ as numerals
- Links: descriptive text, not "click here"
- Code: monospace for commands, files, variable names
- Bold: for UI buttons ("Click **Create**")
- Italic: for emphasis ("Don't forget the **_colon_**")

## Punctuation
- Oxford comma: "tutorials, how-tos, and references"
- Periods in lists: Only if items are full sentences
- Semicolons in bullet points: Only if items are full sentences

## Terminology
- "call" an API (not "hit" or "invoke")
- "set" a value (not "apply" or "assign")
- "deploy" a service (not "push" or "release")
- "authenticate" (not "auth" or "sign in")
- Consistent terminology for key concepts:
  - Choose: "request" vs. "call" vs. "invoke" — pick one
  - Choose: "token" vs. "credential" vs. "secret" — be consistent

## Formatting
### Code Block Language Labels
\`\`\`javascript
// Always specify language for syntax highlighting
\`\`\`

### Callout Boxes
> **Note**: Important but not critical

> **Warning**: High-impact if ignored

> **Tip**: Helpful but optional

## Examples
- Every endpoint: curl + Python + JavaScript
- Every config: YAML example (not just schema)
- Every error: what causes it + how to fix it

## Tools
- Markdown linter: Enforce heading levels, link validity
- Spell check: Catch typos (allow domain-specific terms)
- Style checker: "might" → warn, "click here" → warn
```

---

## Common Gotchas in Technical Writing

**Documentation Debt**: Writing docs feels slower than shipping features; engineers skip it. Then it becomes a massive backlog.
- **Fix**: Make docs part of Definition of Done. Feature isn't shipped until docs are written and reviewed.

**Outdated Examples**: Code changes, API evolves, but docs stay frozen.
- **Fix**: Docs-as-code. Test examples in CI. Docs review as part of pull request process.

**False Clarity**: Explanation sounds clear to you (because you built it) but confuses new people.
- **Fix**: Test on new users. Have someone unfamiliar read your doc and think out loud.

**Incompleteness**: You document the happy path but ignore edge cases.
- **Fix**: Review support tickets monthly. "What did users ask about?" → Document that.

**Scattered Docs**: Docs in Confluence, Notion, GitHub wikis, Google Docs, comments in code.
- **Fix**: Single source of truth. Docs in Git, generated and deployed, one URL.

**No Version Mismatch Between Docs and Code**: Library updated, docs are stale.
- **Fix**: Docs versioned with releases. Release branch X → checkout docs for X.

---

## Further Reading & References

- **Diátaxis Framework**: https://diataxis.fr (recommended reading)
- **Google Developer Docs Style Guide**: https://developers.google.com/style
- **Microsoft Writing Style Guide**: https://docs.microsoft.com/style-guide/
- **Plain Language Guidelines**: https://www.plainlanguage.gov/
- **WritetheDocs Community**: https://www.writethedocs.org/ (conferences, job board, Slack)
- **OpenAPI Specification**: https://swagger.io/specification/ (API docs standard)
