---
name: programming-best-practices
description: Use when writing, changing or reviewing code in any language. Applies TDD, SOLID without speculative abstraction, complete static typing, Google Style docstrings, linting and proper logging instead of print. Trigger on /programming-best-practices, "best practices", "TDD", "SOLID", "typing", "docstring", "lint", "logging", "print".
---

# /programming-best-practices

Write (or review) code following the 6 rules below. Before finishing, walk the 6 checks against the code touched and fix every miss. If invoked with a file, diff or path, audit it and report per rule: OK, FIXED or RISK (with file:line). Never use em dashes.

## 1. TDD
- Load `/tests-guidelines` (Skill tool) and follow it for every test decision: runner detection, test-first cycle, unit, integration and E2E patterns, mocking, and the 80% coverage gate. The bullets below are the summary; where they differ, `/tests-guidelines` wins.
- Write a failing test first, run it and see it fail for the right reason, then write the minimum code to pass it.
- Bug fix: first a test that reproduces the bug, then the fix.
- Refactor only with the tests green before and after.
- Test behavior, not implementation details. Cover the edge cases the code actually branches on.

## 2. SOLID without speculative abstraction
- Single responsibility: one reason to change per function, class or module.
- Depend on abstractions only where there is already a second implementation or a test double that needs it.
- Open/closed, Liskov, interface segregation: apply them when the code already varies along that axis, not in advance.
- No interface with one implementation, no factory for one product, no config for a value that never changes.

## 3. Complete static typing
- Annotate every function parameter and return, and every public attribute (Python type hints, TypeScript explicit types, equivalents elsewhere).
- No `Any` / `any` unless justified in a comment. Prefer precise types: `Literal`, `TypedDict`, `Protocol`, unions, generics.
- Run the project's type checker (mypy, pyright, `tsc --noEmit`) and fix the errors.

## 4. Docstrings in Google Style
- Every public function, class and module gets a docstring: one-line summary, then `Args:`, `Returns:`, `Raises:` (and `Attributes:` for classes) when they apply.
- Describe what and why, not a line-by-line retelling of the code. Keep them in sync when the signature changes.
- Skip trivial private helpers whose name and types already say everything.

## 5. Linting
- Run the project's own linter and formatter (ruff, eslint, golangci-lint, clippy, whatever the repo configures) before concluding.
- Fix every warning in the code you touched. Do not silence a rule with a disable comment unless you state why.
- Do not reformat or fix unrelated files.

## 6. Logging, not print
- Never use `print` / `console.log` for diagnostics or runtime output in application code. Use the language's logging facility (Python `logging`, `logger` in JS/TS, `slog`/`zap` in Go, `tracing` in Rust). `print` stays only for a CLI's actual user-facing output and in throwaway scripts.
- One module-level logger per file (`logger = logging.getLogger(__name__)`). Never call `logging.basicConfig` in library code; configure handlers once, at the application entry point.
- Pick the level on purpose: DEBUG for diagnostics, INFO for normal milestones, WARNING for recoverable oddities, ERROR for failures, CRITICAL for fatal ones. Respect the project's existing level and format conventions.
- Lazy formatting: `logger.info("loaded %s rows", n)`, not f-strings, so the message is only built when the level is enabled.
- In `except` blocks use `logger.exception("...")` (or `exc_info=True`) to keep the traceback; log an error once, where it is handled, not at every layer it passes through.
- Never log secrets, tokens, passwords or personal data (see `/attack-protection`). Prefer structured context (`extra=` / key-value fields) over interpolating whole objects.
- When touching code that already has stray `print` calls, replace those in the lines you change; leave unrelated files alone and mention them.

## Output when writing code
After the code, add one line per rule, e.g. `TDD: /tests-guidelines applied, test_x fails then passes, coverage >= 80% | SOLID: single class, no extra interface | Types: mypy clean | Docs: Google style on public API | Lint: ruff clean | Logging: logger, no print`. Mark a rule not applicable as `n/a`. Anything you could not run or guarantee goes under `RISK` with the reason.
