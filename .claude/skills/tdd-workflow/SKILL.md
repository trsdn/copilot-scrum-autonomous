---
name: tdd-workflow
description: Test-Driven Development workflow - write tests before implementation. Use when implementing features with acceptance criteria, when user says "TDD", "test first", "write tests for this issue", or when starting implementation of a user story.
---

# TDD Workflow Skill

Write failing tests first, then implement code to make them pass.

## When to Use

- Implementing a user story with acceptance criteria
- User explicitly asks for TDD approach
- Building new features with clear requirements
- Refactoring with safety net

## When NOT to Use

- Exploratory prototyping
- UI/visual changes (use visual testing instead)
- One-off scripts
- Documentation changes

## The TDD Cycle

```
┌─────────────────────────────────────────────┐
│  1. RED    │  Write a failing test          │
├─────────────────────────────────────────────┤
│  2. GREEN  │  Write minimal code to pass    │
├─────────────────────────────────────────────┤
│  3. REFACTOR │  Improve code, keep tests green │
└─────────────────────────────────────────────┘
         ↑                                    │
         └────────────────────────────────────┘
```

## Workflow Steps

### Step 1: Parse Acceptance Criteria

Convert each Given/When/Then into a test case:

```markdown
## Acceptance Criteria
- [ ] **Given** user is logged in, **when** they click logout, **then** session is destroyed
```

### Step 2: Write Test File First

Create test file before implementation.

**Test file naming conventions:**

| Language | Pattern | Example |
|----------|---------|---------|
| Python | `test_<module>.py` | `test_logout.py` |
| JavaScript | `<module>.test.js` | `logout.test.js` |
| TypeScript | `<module>.spec.ts` | `logout.spec.ts` |
| Go | `<module>_test.go` | `logout_test.go` |

### Step 3: Run Test (Expect Failure)

```bash
# Python
pytest tests/path/test_module.py -v

# JavaScript
npm test -- --testPathPattern=module

# Go
go test ./path/... -run TestName -v
```

**Expected output:** RED (failing test)

### Step 4: Implement Minimum Code

Write just enough to make the test pass.

### Step 5: Run Test (Expect Pass)

**Expected output:** GREEN (all tests pass)

### Step 6: Refactor

Improve code quality while keeping tests green:
- Extract helper functions
- Improve naming
- Remove duplication
- Add type hints

### Step 7: Repeat for Next Criterion

## Test Structure Template

### Python (pytest)

```python
"""Tests for [feature] - Issue #XXX"""
import pytest
from src.module import function_under_test


class TestFeatureName:
    """Tests for [acceptance criteria group]"""

    @pytest.fixture
    def setup_data(self):
        """Given: [precondition]"""
        return create_test_data()

    def test_when_action_then_result(self, setup_data):
        """
        Given: [precondition]
        When: [action]
        Then: [expected result]
        """
        result = function_under_test(setup_data)
        assert result == expected_value

    def test_edge_case(self, setup_data):
        """Edge case: [description]"""
        pass
```

### JavaScript/TypeScript (Jest)

```typescript
import { functionUnderTest } from '../src/module';

describe('FeatureName', () => {
  describe('when [action]', () => {
    it('should [expected result] given [precondition]', () => {
      const input = setupTestData();
      const result = functionUnderTest(input);
      expect(result).toBe(expectedValue);
    });
  });
});
```

### Go

```go
package pkg

import "testing"

func TestFeatureName(t *testing.T) {
    t.Run("should [result] when [action]", func(t *testing.T) {
        input := setupTestData()
        result := FunctionUnderTest(input)
        if result != expected {
            t.Errorf("got %v, want %v", result, expected)
        }
    })
}
```

## Common Patterns

### Testing Errors

```python
def test_invalid_input_raises_error():
    with pytest.raises(ValueError):
        function_under_test(invalid_input)
```

### Testing Async Code

```python
@pytest.mark.asyncio
async def test_async_operation():
    result = await async_function()
    assert result.success
```

## Checklist Before Implementation

- [ ] All acceptance criteria have corresponding tests
- [ ] Tests are in correct location (`tests/` mirror of `src/`)
- [ ] Test file created before implementation file
- [ ] Tests run and fail for the right reason
- [ ] Edge cases identified and have tests
