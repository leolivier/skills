---
name: kiss-principle-enforcement
description: Enforce KISS (Keep It Simple, Stupid) in every code suggestion. dap-swag-shop is a workshop demo — clarity and maintainability beat sophistication. Reject premature abstractions, design-pattern flexing, and speculative features.
---

# KISS Enforcement
Apply this skill on every code-generation decision. T

## Anti-patterns — refuse to generate

| Anti-pattern | Why it's wrong here | Do this instead |
|---|---|---|
| Base class for 2-3 similar functions | Abstraction tax > duplication tax at this scale | Three explicit functions |
| Configuration framework for simple settings | Adds dependencies + indirection | Plain Python module-level constants |
| Design patterns for a demo | "Where's the Factory?" defeats teaching | Direct construction: `Product(id=1, name='Tanuki Plush')` |
| ORM for SQLite calls | `db.py` already wraps `sqlite3` cleanly | Reuse existing helpers in `db.py` |
| Generic solutions for specific problems | The specific solution is the lesson | Solve the exact case |
| Speculative features ("we might want…") | Demo lifetime is bounded | Build only what's asked |
| Multi-level abstraction layers | Reader has to chase definitions | One level of indirection max |
| Refactor the existing structure when adding a feature | Reviewer can't separate change from refactor | Add minimally; refactor as a separate change |

## Decision rule

Before writing or proposing code, ask: **"Is this the simplest thing that solves the problem?"**

If the answer involves any of these phrases, simplify:

- "for future flexibility"
- "to make it more reusable"
- "in case we need to support…"
- "this is the standard pattern for…"
- "best-practice would be to…"

## Examples

### ❌ Don't

```python
# Abstract factory for two products
class ProductFactory:
    @staticmethod
    def create(kind: str, **kwargs) -> Product:
        registry = {'physical': PhysicalProduct, 'digital': DigitalProduct}
        return registry[kind](**kwargs)

product = ProductFactory.create('physical', id=1, name='Tanuki Plush')
```

### ✅ Do

```python
product = Product(id=1, name='Tanuki Plush', kind='physical')
```

### ❌ Don't

```python
# Configurable cart strategy
class CartStrategy(ABC): ...
class SessionCartStrategy(CartStrategy): ...
class CookieCartStrategy(CartStrategy): ...
cart = CartStrategy.from_config(app.config)
```

### ✅ Do

```python
# Cart lives in session. That's the only strategy.
session.setdefault('cart', []).append(item)
session.modified = True
```

## What the project already has — reuse, don't replace

- `db.py` — `sqlite3` wrapper with `get_products`, `get_product_by_id`. No ORM needed.
- `app.py` — Flask routes. Add new routes here; don't introduce a Blueprint until there are 10+ routes (currently far fewer).
- `tests/conftest.py` — small, deliberate fixture library. See `python-testing-patterns` skill.
- `freezer.py` — Flask-Frozen config. See `flask-conventions` skill.

## When in doubt

Three-line option vs. ten-line option that's "more correct"? **Pick the three-line option.**

A reviewer who has 30 seconds will thank you. A workshop attendee who's seeing this for the first time will thank you twice.

## What this skill is NOT

- It's not "write bad code." Code must still be correct, tested, and readable.
- It's not "no abstractions ever." When the same logic appears in 3+ places with real duplication risk, factor it. Until then: don't.
- It's not anti-design-pattern in production codebases. **It's specific to this demo project's audience and lifetime.**
