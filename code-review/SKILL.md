---
name: code-review
description: Review code changes against this project's standards — CSS/DRY, security, Flask conventions, tests, docs, and CI/CD. Flag approval-gate changes and ask the author to confirm intent before merging.
---

# How to Present Feedback
IMPORTANT: When providing code review feedback:
1. NEVER mention "custom instructions", "according to", or cite instruction sources
2. Present all guidance as your natural understanding and expertise
3. Speak directly about the code issue without referencing these instructions
4. Frame feedback as professional insights, not rule enforcement
5. Mark code smells as CRITICAL violations

# Instructions
* The rules applies to every reviewed file based on the file pattern given in fileFilters for each rule.

## CSS and Frontend Styling (ENHANCED for Code Smell Detection)
fileFilters:
- "static/**/*.css"
- "**/*.css"
### 1. Duplicated CSS Rules (DRY Violations)
**Look for identical or near-identical CSS rules across multiple selectors:**
- Multiple classes with the same properties and values
- Copy-pasted style blocks
- Repeated property declarations

**Example to catch:**
```css
.btn-primary {
  background-color: var(--color-primary);
  color: var(--color-bg-white);
}
.btn-secondary {
  background-color: var(--color-primary);  /* SAME! */
  color: var(--color-bg-white);            /* SAME! */
}
```

**Action:** Flag as code smell and suggest:
- Consolidating selectors: `.btn-primary, .btn-secondary { ... }`
- Creating a base class with shared styles
- Estimated fix time: 5-15 minutes

### 2. Semantic Inconsistencies (Misleading Names)
**Check if class names match their implementation:**
- `.btn-danger` using primary colors instead of danger/red colors
- `.text-secondary` using the same color as `.text-primary`
- `.btn-success` not using success/green colors

**What to check:**
- Look for semantic color variables (--color-danger, --color-success, --color-warning)
- Verify they're used in appropriately named classes
- Flag mismatches between class names and actual colors

**Example to catch:**
```css
:root {
  --color-danger: #cc0000;  /* Red - defined but... */
}
.btn-danger {
  background-color: var(--color-primary);  /* ...not used! */
}
```

**Action:** Flag as semantic mismatch and suggest proper usage

### 3. Unused CSS Variables (Dead Code)
**Find CSS custom properties that are defined but never used:**
- Check :root definitions
- Search for var(--variable-name) usage
- Flag variables defined but not referenced

**Action:** Recommend either:
- Use the variable where appropriate
- Remove if truly unnecessary
- Document why it exists if for future use

### 4. CSS Refactoring Opportunities
**Identify patterns that could be improved:**
- Multiple classes doing the same thing
- Inconsistent naming patterns
- Missing semantic color usage
- Opportunities to leverage design tokens better

### Accessibility & Design Tokens
1. **WCAG Compliance**: Verify contrast ratios meet AA standards (4.5:1)
2. **Design Token Usage**: Check all colors use CSS custom properties (var(--color-*))
3. **Contrast Documentation**: Verify inline comments document contrast ratios
4. **Generic Colors**: Flag any use of generic keywords (orange, red, white, black) outside :root

### CSS Best Practices
1. **DRY Principle**: No duplicated rule sets across different selectors
2. **Semantic Naming**: Class names should reflect purpose and match implementation
3. **Variable Usage**: All defined CSS variables should be used somewhere
4. **Documentation**: Complex styles should include explanatory comments

### Code Smell Severity Guide
- **Critical**: Security or accessibility violations
- **High**: Semantic mismatches affecting UX
- **Medium**: Duplicated code (DRY violations)
- **Low**: Minor improvements or documentation

# Frontend Templates and Assets (ORIGINAL + Enhanced)
fileFilters:
- "templates/**/*.html"
- "static/**/*.js"
## Frontend Code Review Guidelines

### Security
1. **XSS Prevention**: Check HTML templates for proper output escaping
2. **Content Security**: Review inline scripts and external resource loading
3. **Form Validation**: Ensure client-side validation doesn't replace server-side checks

### Accessibility & UX
1. **Semantic HTML**: Verify proper HTML5 semantic elements
2. **ARIA Labels**: Check accessibility attributes for screen readers
3. **Mobile Responsiveness**: Ensure responsive design patterns
4. **GitLab Branding**: Maintain consistent GitLab visual identity

### Template Organization
1. **DRY Principle**: Check for duplicated template code
2. **Base Template**: Ensure proper inheritance from base.html
3. **URL Generation**: Use url_for() instead of hardcoded paths
4. **Static Assets**: Verify proper static file referencing

# Python Django Application Review
fileFilters:
  - "*.py"
  - "!tests/**"
  - "!.venv/**"
## Python Django Code Review Guidelines
### Security & Best Practices
1. **Secret Management**: Flag any hardcoded secrets, API keys, or credentials.
2. **Django Security**: Check for proper session configuration, CSRF protection, and input validation.
3. **SQL Injection**: Review database queries for injection vulnerabilities.
4. **Session Handling**: Verify proper session management and secure cookie settings. Note: the cart lives in `session` by design; orders are not persisted.

### Code Quality
1. **PEP 8 Compliance**: Max line length **120** characters, 4-space indent, f-strings for interpolation.
2. **Always `python3` / `pip3`** — never bare `python` / `pip`. Python 3.14 is required.
3. **Naming**: `snake_case` for functions/variables, `PascalCase` for classes, `UPPER_SNAKE_CASE` for constants.
4. **Imports**: Standard library → third-party → local, in that order. No circular dependencies.

### Performance
1. **Database Queries**: Look for N+1 query problems. Note: the production app reads products from a hardcoded in-memory list in `db.py`, not the SQLite tables.
2. **Memory Usage**: Check for memory leaks in session or global state.
3. **Caching**: Suggest appropriate caching strategies where applicable.

# Test Files
fileFilters:
  - "tests/**/*.py"
  - "test_*.py"
  - "*_test.py"
## Test Code Review Guidelines

### Test Quality
1. **Coverage**: Ensure new code has appropriate test coverage
2. **Test Clarity**: Review test names and structure for readability
3. **Assertions**: Check for meaningful, specific assertions
4. **Test Data**: Verify use of appropriate test fixtures and mock data

### Test Comprehensiveness
1. **Pattern Coverage**: Check if tests cover all variations
 - Example: If testing for 'orange', also test 'red', 'white', 'black'
2. **Edge Cases**: Verify tests include boundary conditions
3. **Negative Tests**: Include tests for what should NOT be present

### Django Testing Patterns
1. **Client Testing**: Use Django test client for route testing
2. **Session Testing**: Test cart and session functionality
3. **Database Testing**: Mock or use test database configurations
4. **Error Scenarios**: Include negative test cases and error handling

# Documentation and Demo Materials
fileFilters:
  - "README.md"
  - "AGENTS.md"
  - "docs/**/*.md"
  - "*.md"
  - "!CLAUDE.md"  # gitignored, never appears in MR diffs
instructions: |
## Documentation Review Guidelines

### Technical Accuracy
1. **Command Accuracy**: Verify all code examples and commands are correct and tested.
2. **Date Precision**: Use specific dates in `YYYY-MM-DD` format. The README "Last Updated" field should be the date of the most recent substantive content change (use the current date at review time, not a hardcoded value).
3. **Color Naming**: Use technically correct color names (e.g., "#ff8c00 = DarkOrange" not "dark orange").
4. **File References**: Ensure all mentioned files and directories exist on disk.
5. **Link Validity**: Verify internal and external links are functional.

### Presentation Quality
1. **Professional Tone**: Maintain appropriate tone for documentation.
2. **Visual Formatting**: Use consistent markdown formatting and emoji usage.
3. **Completeness**: Check for comprehensive coverage of features and capabilities.

# Configuration and CI/CD Files
- name: Configuration and DevOps
fileFilters:
  - "Dockerfile"
  - "docker-compose.yml"
  - "Makefile"
  - "pyproject.toml"
  - ".dockerignore"
  - ".gitignore"
  - ".github/**/*.yaml"
  - ".github/**/*.yml"
## Configuration & DevOps Review Guidelines

### CI/CD Pipelines (`.github/workflows/*`)
1. **Security Scanning**: Ensure security jobs (SAST, Secret Detection, Dependency Scanning, Container Scanning) remain enabled.
3. **Job Dependencies**: Check proper use of `needs:` for fast-fail.
4. **Artifact Management**: Review artifact expiration and size optimization.
6. **Workflow gating**: The pipeline runs only on MRs, protected branches, and tags (see `workflow.rules`). Do not propose unrestricted pipeline triggers.

### Docker Configuration
1. **Image Security**: Use official, minimal base images.
2. **Layer Optimization**: Check for efficient layer caching.
3. **Secrets**: Ensure no secrets in `Dockerfile` or `docker-compose.yml`.

### Dependencies (`pyproject.toml`)
1. **Version Pinning**: Check for appropriate version constraints.
2. **Security**: Flag known vulnerable package versions and propose upgrades.
3. **Minimal Dependencies**: Question unnecessary or duplicate packages.
4. **Approval gate**: Modifying `pyproject.toml` is a high-blast-radius change — ask the MR author to confirm intent before approving package additions or version bumps.

### Build Automation (Makefile)
1. **Command Safety**: Review for proper error handling and sequential execution.
2. **Documentation**: Ensure `help` target describes all available commands.
3. **Environment Consistency**: Always `python3` / `pip3`, `.venv/` (not `venv/`), Python 3.14.

# General Files
fileFilters:
  - "**/*"
  - "!node_modules/**"
  - "!.venv/**"
  - "!build/**"
  - "!public/**"
  - "!__pycache__/**"
  - "!CLAUDE.md"
## General Review Guidelines

### Design Principles (apply to every change)
Self-check before approving: **Is it minimal? Is it simple? Is it unique (DRY)? Is it maintainable?**

1. **KISS (Keep It Simple, Stupid)** — Prefer straightforward, readable logic over clever one-liners. Avoid deep nesting, inheritance towers, and premature optimization. The bar: a junior dev with no prior context can read the change and understand it. Use the kiss skill to assess.
2. **Elegant Minimalism** — Code is a liability, not an asset. Before adding logic, libraries, or abstraction layers, evaluate whether existing code can be simplified or repurposed. Flag dead code, unused variables, redundant comments, and YAGNI scaffolding aggressively.
3. **DRY** — Each piece of knowledge has one authoritative representation. Extract repeated logic into a function or module; centralize magic values into constants/config; updating a rule should require a change in exactly one place.
4. **Maintainability First** — Code is read more than written. Single-responsibility units; intention-revealing names (`calculate_total_order_value`, NOT `calcTotVal`); comments that explain *why* not *what*; no silent `except:`; pure functions and dependency injection where appropriate.

### Code Organization
1. **File Structure**: Maintain clean, logical project organization.
2. **Naming Conventions**: Use clear, descriptive names for files and directories.
3. **Dependencies**: Keep external dependencies minimal and well-justified.

### Approval Gates — Ask Before Modifying
The following changes carry blast radius beyond a single file. Flag them in the review and ask the MR author to confirm explicit intent before approving:

1. **`pyproject.toml`** — especially the deliberately-vulnerable pinned versions (`urllib3==1.26.5`, `Pillow==9.3.0`, `PyYAML==5.3.1`).
2. **`.github/workflows/*.yml`** — pipeline behavior and stage definitions.
5. **Pushes to protected branches** (`main`, `release/*`).

Defer to repository CODEOWNERS for actual approver routing rather than naming individuals here — the `CODEOWNERS` file is the source of truth.


