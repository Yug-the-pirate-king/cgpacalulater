# AI Fix — Issue #5: Cleanup: Remove dead code and unused imports

**Issue body:**

This is an automated issue created by the AI agent to track planned code quality improvements. The AI will fix this in a subsequent run.

---

**AI-proposed fix:**

## Root cause

The project likely accumulated **unused imports** and **dead/commented-out code** during rapid iteration. Without an enforced linter/style check, these artifacts remain in the source files, reducing readability and increasing maintenance noise. They do not affect runtime behavior, but they clutter the codebase and can mask real dependencies.

---

## Actionable fix

Because I do not have direct access to the exact current files, the safest cleanup is to run an automated Python linter/fixer and then manually review any remaining dead code. The repository name `cgpacalulater` strongly suggests a small Python CGPA calculator, so the steps below target Python.

### 1. Identify unused imports and dead code automatically

Run these from the repository root:

```bash
# Install tooling
pip install ruff autoflake

# Auto-remove unused imports
autoflake -ri --remove-all-unused-imports .

# Lint for remaining issues (unused variables, dead code, etc.)
ruff check .
```

### 2. Manually remove obvious dead code

Look for and delete:

- Commented-out print/debug statements
- Unused helper functions
- Duplicate/empty `pass` blocks
- Variables assigned but never read

A representative cleanup diff might look like this:

#### `main.py`

```diff
- import os
- import sys
  import math

  def calculate_cgpa(grades, credits):
      total_points = 0
      total_credits = 0

-     # unused debug variable
-     debug_values = []
-
      for grade, credit in zip(grades, credits):
          point = grade_to_point(grade)
          total_points += point * credit
          total_credits += credit
-         # debug_values.append(point)
-
-     # Dead code: old formula
-     # return total_points / len(grades)
-
      return total_points / total_credits
```

#### `utils.py`

```diff
- import json
- import datetime

  def grade_to_point(grade):
      mapping = {
          "A": 4.0,
          "B": 3.0,
          "C": 2.0,
          "D": 1.0,
          "F": 0.0,
      }
      return mapping.get(grade.upper(), 0.0)
-
- def unused_helper(x):
-     # Leftover from earlier implementation
-     return x * 2
```

### 3. Validate nothing is broken

```bash
# Run any existing tests
pytest

# If no tests, run a quick sanity check
python -m main
```

---

## Follow-up actions

1. **Add a linting CI step** (e.g. GitHub Actions with `ruff` or `flake8`) so dead code/unused imports are caught in pull requests.
2. **Add a pre-commit hook** using `pre-commit` with `ruff` and `autoflake` to prevent future regressions.
3. **Add unit tests** if they do not exist; cleanup becomes much safer when behavior is verified.
4. **Document the cleanup** in the PR description and reference issue #5.

If you share the actual file contents, I can produce the exact patch tailored to the repository.
