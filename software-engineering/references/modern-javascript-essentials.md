---
type: reference
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/reference, plugin/software-engineering, topic/javascript, topic/typescript]
status: active
inspired_by: "Kyle Simpson's *You Don't Know JS Yet* (2nd ed.)"
last_reviewed: 2026-05-09
---

# Modern JavaScript essentials

The mental model for JavaScript every senior engineer should hold, updated through ES2026. Inspired by the *You Don't Know JS Yet* (2nd ed.) book series — same first-principles bias, but kept current with the language as it actually ships in 2026.

Use this as a "load before reasoning about JS" reference for any skill in this plugin that touches application code.

## The three pillars (Simpson's frame)

JavaScript is best understood through three pillars, each with its own surprising depth:

1. **Scope and closures** — where variables live, when they live, and how inner functions capture outer state. Lexical scope is the foundation; everything from module patterns to React hooks rests on it.
2. **Prototypes (and class as syntax)** — the `class` keyword is a thin layer over prototype delegation. Knowing the underlying mechanism explains every weird `this` bug.
3. **Types and coercion** — JS has types (the spec lists them), the runtime checks them, and coercion is intentional, not a bug. Predicting `==` vs `===` requires the abstract equality algorithm in your head.

If a JS bug feels mysterious, it's almost always a misread on one of those three. Reach for the relevant pillar before reaching for Stack Overflow.

## Scope and closures

**Lexical scope** is determined at author time, not call time. A function's accessible variables are the ones in scope where the function was defined.

```ts
function makeCounter() {
  let count = 0;
  return () => ++count;
}
const next = makeCounter();
next(); // 1
next(); // 2 — `count` is held alive by the closure
```

Patterns that rely on this:
- React hooks (closures over render-scoped state).
- Module pattern, IIFE.
- Memoization, debounce, throttle, observer wiring.

**Block scope** (`let`, `const`) vs **function scope** (`var`) — `var` is hoisted to the function, `let`/`const` to the block but with a temporal dead zone. Default to `const`. Reach for `let` only when reassignment is part of the contract. `var` has no place in 2026 code.

**Closures and memory** — closures keep their captured frame alive. A long-lived closure over a large object means that object never GC's. Watch for this in event handlers and timers attached at startup.

## `this` and prototypes

`this` is bound at **call** time, not at definition. Five rules in priority order:

1. `new fn()` → `this` is the new instance.
2. `fn.call(ctx, ...)` / `fn.apply(ctx, ...)` / `fn.bind(ctx)()` → explicit `ctx`.
3. `obj.fn()` → `this` is `obj`.
4. Default → `undefined` in strict mode, the global object otherwise.
5. **Arrow functions ignore all of the above** — `this` is lexical, inherited from the enclosing scope.

Default to arrow functions for callbacks (handlers, predicates, `setTimeout` bodies). Use `function` declarations when you need dynamic `this` (rarely) or a named function for readable stack traces.

`class` is sugar over prototype delegation:

```ts
class Animal {
  constructor(public name: string) {}
  speak() { return `${this.name} makes a sound`; }
}
class Dog extends Animal {
  speak() { return `${this.name} barks`; }
}
```

`Dog.prototype.__proto__ === Animal.prototype`. Method lookup walks the prototype chain. Override by re-declaring; call up with `super.speak()`.

## Types, coercion, and equality

JavaScript has primitive types: `undefined`, `null`, `boolean`, `number`, `bigint`, `string`, `symbol`. Everything else is an `object` (including `Array`, `Function`, `Date`, `Map`, `Set`, `Promise`).

**Coercion** is the algorithm that converts between types. It's intentional and documented:
- `==` performs abstract equality (with coercion).
- `===` performs strict equality (no coercion).
- `Object.is(a, b)` is strict + correctly handles `NaN` and `+0` / `-0`.

Defaults:
- Use `===` for everything except deliberate coercion (`x == null` to test "is `null` or `undefined`" is a known idiom).
- For floats, never `===` — use `Math.abs(a - b) < EPSILON` or a helper.
- For object equality, write a comparator or use a library; reference equality is rarely what you want.

**Boxing**: primitives autobox into their object wrappers (`String`, `Number`, `Boolean`) for method calls (`"hi".toUpperCase()`). Don't construct wrapper objects with `new String("hi")` — they're truthy and break `===`.

## Asynchrony

Three eras of async patterns coexist in production code; know all three:

1. **Callbacks** — every event-driven API in Node.js was built on these. Inversion of control is the cost.
2. **Promises** — first-class values that resolve later. Compose with `.then`, `.catch`, `.finally`. Critical: a `Promise` is *not* its resolved value; you await it.
3. **`async`/`await`** — sugar over promises with try/catch. Default to this for new code.

Modern primitives:
- `Promise.all([a, b])` — wait for all; reject on first failure.
- `Promise.allSettled([a, b])` — wait for all; never rejects.
- `Promise.race([a, b])` — first to settle wins.
- `Promise.any([a, b])` — first to fulfill wins; rejects only if all reject.
- **Top-level `await`** in ES modules — works in Node, native browsers, bundlers.

**The microtask queue**: `await` resumes on the microtask queue, which runs before the next macrotask (timers, I/O). This means `await Promise.resolve()` is *fast* — it doesn't yield to the event loop the way `setTimeout(0)` does.

## Modules

ES modules (`.mjs` or `"type": "module"`) are now the universal target. CommonJS (`require`) is legacy but still common in Node tooling and old packages.

```ts
// foo.ts
export function add(a: number, b: number) { return a + b; }
export default class Foo { /* ... */ }

// bar.ts
import Foo, { add } from "./foo";
```

Notes:
- Imports are **live bindings**, not copies. If `foo` exports a `let count`, the importer sees the latest value.
- `import.meta.url` for module-relative paths.
- **JSON modules** (ES2025): `import data from "./config.json" with { type: "json" };` — ships in Node 22+ and modern browsers.
- **Import attributes** (`with`) replaced the old `assert` syntax.

## Iteration

Iterables are objects with a `[Symbol.iterator]()` method that returns an iterator. `Array`, `Map`, `Set`, `String`, generators are all iterable. `for...of` consumes any iterable.

**Iterator helpers (ES2025)** — first-class `.map`, `.filter`, `.take`, `.drop`, `.reduce`, `.toArray` on iterators themselves, lazily evaluated:

```ts
const firstThreeEvens = naturals()
  .filter(n => n % 2 === 0)
  .take(3)
  .toArray();
```

This finally lets you compose lazy pipelines without a third-party library. Expect to see this replace eager `Array.from(...).filter(...).slice(...)` patterns.

**`Iterator.from(x)`** lifts any iterable into the helper-rich type.
**`Iterator.concat(a, b, ...)`** (ES2026) concatenates iterables lazily.

## Set, Map, and the new convenience methods

**Set methods (ES2025)** — long-overdue mathematical operations:
- `a.union(b)`, `a.intersection(b)`, `a.difference(b)`, `a.symmetricDifference(b)`
- `a.isSubsetOf(b)`, `a.isSupersetOf(b)`, `a.isDisjointFrom(b)`

```ts
const A = new Set([1, 2, 3]);
const B = new Set([2, 3, 4]);
A.intersection(B); // Set {2, 3}
```

Use `Map` over a plain object when keys are not strings, when iteration order matters, or when you need a frequent `size` count. Use `Set` over an array when membership tests are the dominant operation.

`WeakMap` and `WeakSet` for caches keyed on objects you don't want to keep alive.

## Dates: prefer `Temporal`

`Date` has been broken since 1995 — mutable, locale-fragile, no time-zone primitives. **`Temporal` reached Stage 4 at TC39's March 2026 meeting** and ships in ES2026.

```ts
const now = Temporal.Now.zonedDateTimeISO("America/New_York");
const inAYear = now.add({ years: 1 });
const dur = Temporal.Duration.from({ hours: 3, minutes: 30 });
```

Types:
- `Temporal.Instant` — exact moment, no zone.
- `Temporal.ZonedDateTime` — moment in a specific time zone.
- `Temporal.PlainDate`, `PlainTime`, `PlainDateTime` — wall-clock without zone.
- `Temporal.Duration` — first-class duration arithmetic.

For new code in 2026, default to `Temporal`. Polyfill (`@js-temporal/polyfill`) on older runtimes; native in Node 24+ and current browsers.

## TypeScript fundamentals (assumed default)

In this plugin, TS strict mode is the default. The core mental model:

- **Structural typing** — types match by shape, not name.
- **Inference** — let the compiler infer where it can; annotate at boundaries (function signatures, exports, public APIs).
- **`unknown` over `any`** — `unknown` forces a type narrowing before use.
- **Discriminated unions** for state machines and result types:

```ts
type Result<T, E> = { ok: true; value: T } | { ok: false; error: E };
```

- **`as const`** for literal narrowing (`["GET", "POST"] as const` becomes `readonly ["GET", "POST"]`).
- **`satisfies`** for "this expression must conform to T but keep its narrow type."
- **Generics with constraints**: `<T extends Foo>` reads "T must be assignable to Foo."

Avoid:
- `any` outside of escape hatches you've documented.
- Type assertions (`as Foo`) when narrowing would work.
- Decorators in production code unless you have a specific framework requirement.

## Common failure modes

- **Reaching for `var` or function-scoped patterns** — inherited from old codebases; rewrite with `const`/`let` and block scope.
- **Missing `await`** — silently returns a `Promise<T>` and downstream code thinks it has the value.
- **Mutating React/Redux state** — JS lets you, the framework breaks. Default to immutable updates (`...spread`, `Map.set` returns new copy with Immer, `structuredClone`).
- **Using `JSON.parse(JSON.stringify(x))` for deep copy** — drops `Date`, `Map`, `Set`, `undefined`. Use `structuredClone(x)` (ES2022, native everywhere now).
- **Confusing `Promise<T>` with `T`** — TS often catches this; eslint's `no-floating-promises` is a must-have.
- **Date math with `Date`** — DST, time zones, leap seconds will bite you. Use `Temporal`.

## When to reach beyond this reference

- For deep dives on a specific pillar, read the matching *YDKJS Yet* book ([Get Started](https://github.com/getify/You-Dont-Know-JS), Scope & Closures, Objects & Classes, Types & Grammar). The mental models age well even when the syntax updates.
- For the formal language spec, [tc39.es/ecma262](https://tc39.es/ecma262/) is the source of truth.
- For TypeScript-specific patterns in our Next.js stack, see `nextjs-typescript-best-practices.md` inside `software-engineering-nextjs-routing/references/`.
