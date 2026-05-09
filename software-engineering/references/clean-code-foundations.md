---
type: reference
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/reference, plugin/software-engineering, scope/foundational, topic/software]
status: active
---
# Clean Code Foundations

Code-level best practices that apply across every software-engineering skill in this plugin. Organized around the principles in Robert C. Martin's *Clean Code*, condensed into six topic areas with the "what / why / how" for each principle.

These are foundational — they apply to frontend, backend, mobile, data, infra, and any other language or framework. Reference them when writing or reviewing code, doing code review, and especially when establishing team-level conventions.

Source synthesis: Robert C. Martin's *Clean Code*, plus the curated practitioner summaries from [@s4.codes](https://www.instagram.com/s4.codes/) (Asfar Ali, Senior Software Engineer).

---

## 1. Meaningful Names

Names are the most-read part of any codebase. Every name is a tiny essay — make it carry weight.

### 1.1 Use intention-revealing names

Names should answer three questions automatically: *Why does this exist? What does it do? How is it used?*

```javascript
// ❌ Reveals nothing
let d;             // elapsed time in days
let list1 = [];

// ✅ Reveals intent
let elapsedTimeInDays;
let flaggedCells = [];
```

If a name needs a comment to explain it, the name has failed.

### 1.2 Avoid disinformation

Don't use names that mislead. Avoid words that have meaning in the domain or industry but mean something different in your code.

- Don't call something a `List` if it's not a `List` — call it a `Group` or `Bag` or whatever it actually is.
- Don't use lowercase `l` or uppercase `O` in names — they look like `1` and `0`.
- Don't use similar names for similar concepts that differ subtly. `XYZControllerForEfficientHandlingOfStrings` and `XYZControllerForEfficientStorageOfStrings` are pure cognitive load.

### 1.3 Make meaningful distinctions

If two things are different, the names must show how. Avoid noise words: `data`, `info`, `the`, `a`, `an`, `Object`, `Manager`, `Processor`, `getActiveAccount` vs `getActiveAccounts` vs `getActiveAccountInfo`.

```python
# ❌ Indistinguishable
def get_active_account()
def get_active_accounts()
def get_active_account_info()

# ✅ Real distinction
def find_active_account()       # returns one
def list_active_accounts()      # returns all
def get_active_account_summary() # returns aggregate metadata
```

### 1.4 Use pronounceable names

You'll talk about this code in meetings. If your variable is `genymdhms` ("generation date, year, month, day, hour, minute, second"), nobody can say it out loud.

```java
// ❌
private Date genymdhms;
private Date modymdhms;

// ✅
private Date generationTimestamp;
private Date modificationTimestamp;
```

### 1.5 Use searchable names

Single-letter names and magic numbers are unsearchable. Reserve `i`, `j`, `k` for tight local scopes (a few-line `for` loop). Anything that lives longer than 5 lines deserves a real name.

```javascript
// ❌
for (let j = 0; j < 34; j++) {
  s += (t[j] * 4) / 5;
}

// ✅
const WORK_DAYS_PER_WEEK = 5;
const REAL_DAYS_PER_IDEAL_DAY = 4;
const NUMBER_OF_TASKS = 34;
let sum = 0;
for (let taskIndex = 0; taskIndex < NUMBER_OF_TASKS; taskIndex++) {
  const realTaskDays = (taskEstimate[taskIndex] * REAL_DAYS_PER_IDEAL_DAY) / WORK_DAYS_PER_WEEK;
  sum += realTaskDays;
}
```

### 1.6 Avoid encodings

Don't encode type information into names (Hungarian notation: `strName`, `iCount`). Modern languages have type systems; let them do the work. Don't prefix members with `m_`. Don't prefix interfaces with `I`.

```typescript
// ❌
class Account {
  private m_dsc: string;
  setDsc(s_str: string) { this.m_dsc = s_str; }
}

// ✅
class Account {
  private description: string;
  setDescription(description: string) { this.description = description; }
}
```

### 1.7 Avoid mental mapping

Readers shouldn't have to translate your names into known concepts. `r` is shorter than `url` but everyone reading has to remember "r means url here." That tax compounds across a codebase.

> **Rule of thumb**: Clarity > brevity. A name that takes 1 extra second to type saves hours of reading.

---

## 2. Functions

Functions are the verbs of your codebase. The right shape makes everything else easier; the wrong shape makes everything else harder.

### 2.1 Small. Then smaller.

Functions should be **short**. The original Clean Code rule: 20 lines is too long. Most should be 4–10 lines.

The reason isn't aesthetic — it's that small functions have **one reason to change**, are **easy to name**, and are **easy to test**.

```python
# ❌ 50-line function doing 6 things
def process_user_signup(form_data):
    # validate email
    # validate password
    # check uniqueness
    # hash password
    # create user record
    # send welcome email
    ...

# ✅ Small functions, named for intent
def process_user_signup(form_data):
    validate_signup_form(form_data)
    ensure_email_is_unique(form_data.email)
    user = create_user(form_data)
    send_welcome_email(user)
    return user
```

### 2.2 Do one thing

Each function should do one thing, do it well, and do it only. The test: can you describe the function in a sentence with no "and"s or "if"s?

If a function has multiple sections separated by blank lines, those sections are different things. Extract them.

### 2.3 One level of abstraction per function

Mixing high-level intent with low-level mechanics inside the same function is the most common readability failure.

```javascript
// ❌ Mixed levels
function renderInvoice(invoice) {
  const html = `<h1>${invoice.id}</h1>`;
  for (const item of invoice.items) {
    // 30 lines of HTML building
  }
  // 20 more lines of footer building
  return html;
}

// ✅ Each function operates at one level
function renderInvoice(invoice) {
  return [
    renderInvoiceHeader(invoice),
    renderInvoiceItems(invoice.items),
    renderInvoiceFooter(invoice),
  ].join('');
}
```

### 2.4 The Stepdown Rule (reading flow)

Code should read top-down like a newspaper. Highest-level function at the top, every function definition appears below its first caller, related functions stay grouped.

When you can read a file top-to-bottom and understand it without jumping around, the structure is right.

### 2.5 Prefer fewer arguments

- 0 arguments (niladic): ideal
- 1 argument (monadic): good
- 2 arguments (dyadic): acceptable, but harder to test
- 3 arguments (triadic): avoid; needs strong justification
- 4+ arguments (polyadic): refactor

If you need many arguments, several of them probably belong together as a struct/object.

```python
# ❌
def create_circle(x, y, radius, color, fill, stroke_width):
    ...

# ✅
def create_circle(center: Point, radius: float, style: CircleStyle):
    ...
```

### 2.6 No flag arguments

Boolean parameters mean the function does two things — refactor into two functions.

```python
# ❌
def render(data, isProduction):
    if isProduction:
        # produce minified output
    else:
        # produce dev output

# ✅
def render_for_production(data): ...
def render_for_development(data): ...
```

### 2.7 Avoid side effects

Functions should do what their name promises and nothing else. A function called `checkPassword(user, password)` should not also log the user in. Hidden side effects are a major source of bugs.

If side effects are necessary, name the function so callers are aware: `checkPasswordAndInitializeSession(...)` (better: split into two functions).

### 2.8 Command-Query Separation

A function should either *do something* (command) or *answer something* (query) — never both.

```javascript
// ❌ Does and answers
if (set("username", "alice")) { ... }  // Did it set? Or check that it was set?

// ✅ Separated
if (attributeExists("username")) {
  setAttribute("username", "alice");
}
```

### 2.9 Prefer exceptions to error codes

Returning error codes forces callers to deal with errors in-line, polluting the happy path. Throwing exceptions lets the happy path stay clean and error handling stay contained.

```python
# ❌
result = delete_page(page)
if result == E_OK:
    log_info("Page deleted")
else:
    log_error(f"Delete failed: {result}")

# ✅
try:
    delete_page(page)
    log_info("Page deleted")
except DeleteError as e:
    log_error(f"Delete failed: {e}")
```

### 2.10 Don't repeat yourself

Duplicated code is the source of most bugs. When a change has to happen in three places, two will be forgotten. Extract shared logic into a single function. Do this aggressively.

### 2.11 The Writing Process

You don't write clean functions on the first pass. You write the function long, ugly, and working — then you refactor:

1. Get it working with tests
2. Extract sub-functions until each one does one thing
3. Rename until intent is obvious
4. Reduce arguments until each function is simple

Clean code is *rewritten* code, not first-draft code.

---

## 3. Comments

Comments are an apology for failing to express intent in the code. Most comments are bad. Good comments are precise and earn their place.

### 3.1 Explain yourself in code

The first move when you feel like writing a comment: rename the variable, extract the function, restructure the conditional. **Self-documenting code beats documented code every time.**

```python
# ❌ Comment explaining what the code does
# Check if the employee is eligible for full benefits
if (employee.flags & HOURLY_FLAG) and (employee.age > 65):
    ...

# ✅ The code says it
if employee.is_eligible_for_full_benefits():
    ...
```

### 3.2 Good comments

Some comments earn their place:

- **Legal comments** (copyright, license headers)
- **Informative comments** that can't be expressed in code (e.g., the format string for a regex date matcher)
- **Explanation of intent** when the *why* isn't obvious from the code
- **Warnings of consequences** ("Don't run this without locking; the test takes 4 hours")
- **TODO comments** that genuinely will be addressed (and tracked)
- **Public API documentation** (JSDoc, docstrings) — different rules apply: the world reads the docs, your team reads the code

### 3.3 Bad comments

Most comments are bad. Recognize and remove:

- **Mumbling** — vague comments that don't help: `// todo, fix later`
- **Redundant comments** — restating what the code obviously does
- **Misleading comments** — once accurate, now lying as code drifted
- **Mandated comments** — `@param`, `@return` for every method even when redundant
- **Journal comments** — change history (use git)
- **Noise comments** — `// default constructor`
- **Position markers** — `// ////////// Actions /////////`
- **Closing brace comments** — `} // end while`
- **Attributions and bylines** — git knows who wrote it
- **Commented-out code** — delete it; git remembers
- **HTML in source comments** — clutters editing
- **Too much information** — historical discussions belong elsewhere
- **Inobvious connection** — a comment whose relationship to the code isn't clear

### 3.4 The refactoring principle

Most "I need a comment here" moments are signals to refactor:

- Need to explain what a variable means? **Rename it.**
- Need to explain what a block of code does? **Extract a function with that name.**
- Need to explain why a condition is true? **Extract it into a method with a self-explaining name.**

```python
# ❌ Comment as crutch
# Bypass paid users for the trial extension
if user.subscription_status == "active" and user.tier in ("paid", "premium"):
    return False

# ✅ Code as documentation
if user.is_paid_subscriber():
    return False
```

### 3.5 When comments protect

Some comments exist to prevent well-meaning future changes:

```python
# Sequence matters — auth must happen before logging,
# or logged session IDs will be unauthenticated and meaningless.
authenticate(request)
log_request(request)
```

This is a comment that earns its keep. Without it, someone reorders these for "performance" and breaks security.

### 3.6 Code tells you how, comments tell you why

```python
# Retry with exponential backoff because the upstream API rate-limits
# unpredictably during their nightly batch (~3am UTC).
for attempt in range(MAX_RETRIES):
    try:
        return call_upstream()
    except RateLimitError:
        time.sleep(2 ** attempt)
```

The code shows the *how*. The comment captures the *why* — context the next reader can't derive from the code alone.

---

## 4. Formatting

Formatting is communication. The shape of code on the page is the first thing readers see.

### 4.1 Vertical formatting — code reads top to bottom

- **File length**: aim for 200–500 lines. Shorter is usually clearer.
- **Newspaper structure**: most important / highest-level at the top, details below.
- **Vertical openness between concepts**: blank lines separate complete thoughts. Inside a method, blank lines separate phases.
- **Vertical density within concepts**: lines that belong together should be packed together; separations imply distinct concepts.

### 4.2 Vertical distance — keep related things close

- **Variable declarations** as close to their first use as possible
- **Instance variables** in one place at the top of the class
- **Dependent functions** vertically close, with the caller above the callee
- **Conceptually similar functions** grouped, even if there's no direct dependency

### 4.3 Horizontal formatting — code reads left to right

- **Line length**: 80–120 characters. The exact number is less important than picking one and enforcing it.
- **Horizontal openness around operators**: `a = b + c;` not `a=b+c;`
- **Horizontal density**: no spaces inside parentheses for function calls — `getName()` not `getName( )`.
- **Indentation matters**: it shows scope. Don't break it for one-liners.

### 4.4 Team rules

There is no objectively correct formatting style. There is *team-consistent* formatting. Pick the rules, encode them in the linter/formatter, and stop arguing.

> **Use Prettier, Black, gofmt, rustfmt, etc. Configure once, never debate again.**

---

## 5. Objects and Data Structures

The fundamental tension: **objects expose behavior and hide data**; **data structures expose data and have no behavior**. Mixing them creates the worst of both worlds.

### 5.1 Data abstraction

Objects should expose *behavior* (what you can do) and hide *data* (how it's stored). Don't just wrap fields in getters/setters and call it encapsulation.

```javascript
// ❌ Concrete (exposes implementation)
class Vehicle {
  getFuelTankCapacityInGallons() { ... }
  getGallonsOfGasoline() { ... }
}
const remaining = v.getGallonsOfGasoline() / v.getFuelTankCapacityInGallons();

// ✅ Abstract (exposes meaning)
class Vehicle {
  getPercentFuelRemaining() { ... }
}
const remaining = v.getPercentFuelRemaining();
```

### 5.2 Data/Object Anti-Symmetry

- **Procedural code** (using data structures) makes it easy to add new functions without changing existing data structures.
- **Object-oriented code** makes it easy to add new classes without changing existing functions.

The complement: procedural code makes adding new data structures hard; OO makes adding new functions hard.

Pick the right tool for the change axis you expect. **Don't use OO when procedural is simpler. Don't use procedural when OO is simpler.**

### 5.3 The Law of Demeter

A method on object `O` should only call methods on:
- `O` itself
- An object passed in as a parameter
- An object `O` creates
- A direct component of `O`

In short: **don't talk to strangers**. Don't chain through unrelated intermediaries.

```python
# ❌ Train wreck — knows too much about the structure
final_path = ctxt.get_options().get_scratch_dir().get_absolute_path()

# ✅ Tell, don't ask — let ctxt give you what you need
final_path = ctxt.get_absolute_scratch_path()
```

The train-wreck form binds your code to the entire structure of `ctxt`. The corrected form binds only to one method.

### 5.4 Hybrids, DTOs, and Active Records

- **Hybrids** (half object, half data structure) are the worst of both worlds. They have getters and setters AND business logic. Avoid.
- **DTOs (Data Transfer Objects)** are pure data structures — public fields, no behavior. Used at boundaries (DB rows, API payloads). Don't add behavior to them.
- **Active Records** are DTOs with `save()`/`find()` methods. They're a special-case DTO. Don't add domain logic to them — that belongs in proper objects that *use* the active record.

Pick a side. Your class can't be both an object and a data structure.

---

## 6. Error Handling

Error handling is one thing — it should never obscure what the function is actually doing.

### 6.1 Use exceptions, not return codes

Return codes pollute every caller with error checking. Exceptions let the happy path stay clean.

```python
# ❌
result = delete_record(id)
if result == ERR_NOT_FOUND:
    log_warning("Not found")
elif result == ERR_LOCKED:
    log_error("Locked")
elif result == OK:
    log_info("Deleted")

# ✅
try:
    delete_record(id)
    log_info("Deleted")
except RecordNotFound:
    log_warning("Not found")
except RecordLocked as e:
    log_error(f"Locked: {e}")
```

### 6.2 Write try-catch first

When implementing logic that can fail, **write the try-catch block first**, then fill in the body. This forces you to think about error states up-front rather than bolt them on later.

The `try` block is a transaction. The `catch` block is what to do if the transaction fails. Both should be clean and well-named.

### 6.3 Use unchecked exceptions (where the language supports them)

In Java specifically, checked exceptions force every caller in the chain to declare or catch — they violate the Open/Closed Principle. Most modern Java code uses runtime (unchecked) exceptions. Other languages mostly avoid the distinction.

### 6.4 Provide context with exceptions

Every exception should carry enough information to know:
- What operation failed
- What inputs caused the failure
- What the caller might do about it

```python
# ❌
raise ValueError()

# ✅
raise InvalidInvoiceTotal(
    f"Invoice {invoice.id} total {total} exceeds account credit limit {limit}"
)
```

### 6.5 Define exceptions in terms of the caller's needs

Wrap third-party exceptions into your own domain exceptions. This means callers depend on your exceptions, not the third-party's. When you swap the third-party library, no caller code changes.

```python
# ✅ Wrap external exception
try:
    response = stripe.charges.create(...)
except stripe.error.CardError as e:
    raise PaymentDeclined(reason=e.user_message) from e
```

### 6.6 Don't return null. Don't pass null.

`null` is a special case that callers must handle defensively, and they often forget. Prefer:

- **Return empty collections**, not null.
- **Return Optional / Maybe / Result types** when expressing absence.
- **Throw exceptions** when the absence is exceptional.
- **Use null-object patterns** when "missing" has consistent default behavior.

For arguments: design APIs so null is never a valid input. Validate early.

---

## 7. Putting it together — a quick decision guide

When writing code, ask in this order:

1. **Naming**: Does every name reveal intent? Could it be misread?
2. **Functions**: Is each function small, doing one thing, at one level of abstraction?
3. **Side effects**: Does the function do anything its name doesn't promise?
4. **Comments**: Could this comment be replaced by a better name or extracted function?
5. **Errors**: Does error handling stay out of the way of normal logic?
6. **Coupling**: Am I reaching through chains of objects? Am I exposing data instead of behavior?
7. **Formatting**: Is this file consistent with team rules? Does it read top-to-bottom like a newspaper?

When reviewing code, ask:

1. **Could a new team member read this and understand it?**
2. **If I changed one thing, would the change be local?**
3. **Are the abstractions honest? Do names match behavior?**
4. **Are there hidden side effects?**
5. **Does the code show what's important by what's at the top of the file?**

---

## How this reference is used

Every skill in the `software-engineering` plugin can reference this file when:
- Writing or reviewing production code
- Establishing team coding standards
- Doing code review or debugging
- Designing APIs (function signatures, error handling, data shape)
- Onboarding a developer to a codebase

Specific skills lean on specific sections:

| Section | Most relevant skills |
|---|---|
| Meaningful Names | All — universal |
| Functions | All — universal |
| Comments | `technical-writer`, all engineering skills |
| Formatting | `frontend-developer`, `design-system-engineer`, all language-specific skills |
| Objects & Data | `backend-architect`, `frontend-developer`, `data-engineer` |
| Error Handling | `backend-architect`, all engineering skills |

---

## Source

These principles synthesize:

- Robert C. Martin, *Clean Code* (2008) — the canonical text
- The [@s4.codes](https://www.instagram.com/s4.codes/) curated practitioner posts (Asfar Ali) — modern condensed examples
- Decades of observed practice across the industry

When in doubt about whether code is clean, the test that beats every formal metric is: **can a teammate read this six months from now and understand it without you in the room?**
