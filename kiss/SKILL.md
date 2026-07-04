---
name: kiss-principle-enforcement
description: Enforce KISS (Keep It Simple, Stupid) in every code suggestion. dap-swag-shop is a workshop demo — clarity and maintainability beat sophistication. Reject premature abstractions, design-pattern flexing, and speculative features.
---

# 🎯 Overarching Design Principle: KISS

Keep It Simple, Stupid - All code generation and suggestions MUST follow the KISS principle.

## When Generating Code

    Choose the simplest solution that solves the problem
    Avoid over-engineering - Don't add unnecessary abstraction layers
    Prefer explicit over clever - Clear code beats compact code
    No speculative features - Only build what's explicitly requested
    Minimal dependencies - Don't suggest new libraries unless essential

## Decision rule

Before writing or proposing code, ask: **"Is this the simplest thing that solves the problem?"**

If the answer involves any of these phrases, simplify:

- "for future flexibility"
- "to make it more reusable"
- "in case we need to support…"
- "this is the standard pattern for…"
- "best-practice would be to…"

## Red Flags to Avoid

* Creating base classes or abstract factories for 2-3 similar functions
* Adding configuration frameworks for simple settings
* Implementing design patterns that add complexity without clear benefit
* Suggesting ORMs or complex data layers for simple database operations
* Building generic solutions when specific ones are clearer

Remember: Maintainability and clarity trump sophistication.

## Companion Principles (apply alongside KISS)

* Elegant Minimalism — Code is a liability, not an asset. Before adding logic, libraries, or abstraction layers, check whether existing code can be simplified or  repurposed. Remove dead code, unused variables, and YAGNI scaffolding aggressively.
* DRY (Don't Repeat Yourself) — Each piece of knowledge — logic, business rule, configuration — has one authoritative location. Extract repeated logic, centralize magic values, ensure rule changes happen in exactly one place.
* Maintainability First — Code is read more than written. Single-responsibility units, intention-revealing names (calculate_total_order_value, never calcTotVal), comments that explain why not what, no silent except:, and testable design (pure functions, dependency injection).

Self-check before output: Is it minimal? Is it simple? Is it unique (DRY)? Is it maintainable?


## When in doubt

Three-line option vs. ten-line option that's "more correct"? **Pick the three-line option.**

A reviewer who has 30 seconds will thank you. A workshop attendee who's seeing this for the first time will thank you twice.

## What this skill is NOT

- It's not "write bad code." Code must still be correct, tested, and readable.
- It's not "no abstractions ever." When the same logic appears in 3+ places with real duplication risk, factor it. Until then: don't.
- It's not anti-design-pattern in production codebases. **It's specific to this project's audience and lifetime.**
