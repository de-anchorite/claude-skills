---
name: tdd
description: "Test-first development rules: red-green-refactor, behaviour-based testing, xUnit with Testcontainers, GivenA builders, and WhenBehaviour naming. Apply to all coding tasks."
---

# Test-Driven Development

All code is written test-first. These rules are non-negotiable and apply to every coding task.

## Trigger conditions

Apply this skill to all coding tasks involving implementation, tests, or refactoring.

## Core rules

1. **Always start with a test.** Never write implementation before a failing test requires it.
2. **Test behaviours, not internals.** No private methods, internal state, or implementation details.
3. **One assertion per test.** Split if you need more.
4. **Implement only what the test requires.** Minimum code to pass — nothing more.
5. **Write the next test before implementing further behaviour.**
6. **Red → Green → Refactor. Always.**

## The cycle

```
PLAN     → Write test plan before any code (see Planning section)
RED      → Write a failing test for the next behaviour
GREEN    → Write minimum code to pass it
REFACTOR → Clean up with all tests green; no new behaviour
REVIEW   → Stop. Show diff to user. Await approval.
COMMIT   → git commit accepted changes; mark cycle done in plan
```

Cycles are short — minutes, not hours.

## Planning

Before any code, write a test plan to `.claude/plans/tdd-<feature-name>.md`. Format:

```markdown
# TDD Plan: <feature name>

## Test cases
- [ ] Simplest happy path: <description>
- [ ] Happy path: <description>
- [ ] ...
- [ ] Unhappy path: <description>
- [ ] ...
- [ ] Edge case: <description>
- [ ] ...
- [ ] Corner case: <description>
- [ ] ...
```

Order cases following the Sequencing section. After each committed cycle, update the plan — mark the completed case `[x]` and save the file.

## Review and commit

After each RED → GREEN → REFACTOR cycle:

1. **Stop.** Do not proceed to the next test case.
2. **Show the user** a summary of what changed (files modified, test name, what behaviour was added).
3. **Wait for explicit approval** before committing. If the user requests changes, make them and re-show.
4. **On approval**, run:
   ```
   git add <changed files>
   git commit -m "<conventional commit message describing the behaviour added>"
   ```
5. **Mark the test case `[x]`** in `.claude/plans/tdd-<feature-name>.md` and save it.
6. Proceed to the next test case.

## Stubbing new methods

When the first test against a new method won't compile, stub immediately so it can run and fail:
- Returns a value → `throw new NotImplementedException()`
- Void → empty body

Do not implement any real logic at stub stage.

## Test naming

- Suite: `WhenDoingABehaviour`
- Test: `The_outcome_described_in_plain_english`

```csharp
public class WhenAddingAUser
{
    [Fact]
    public async Task The_user_is_created_against_the_correct_account() { }

    [Fact]
    public async Task A_duplicate_email_returns_a_conflict_response() { }
}
```

## Assertions

Prefer **FluentAssertions** (NuGet: `FluentAssertions`) over bare xUnit `Assert.*`.

```csharp
result.Should().Be(42);
user.Should().NotBeNull();
response.StatusCode.Should().Be(HttpStatusCode.Conflict);
users.Should().ContainSingle(u => u.Email == "test@example.com");
act.Should().ThrowAsync<ValidationException>();
```

## Fakes, not mocks

Use in-memory implementations. Never use Moq or NSubstitute.

```csharp
public class InMemoryUserRepository : IUserRepository
{
    private readonly List<User> _users = new();

    public Task Save(User user)
    {
        _users.RemoveAll(u => u.Id == user.Id);
        _users.Add(user);
        return Task.CompletedTask;
    }

    public Task<User?> GetById(string id) =>
        Task.FromResult(_users.FirstOrDefault(u => u.Id == id));
}
```

## Builders (Given pattern)

When setup recurs, extract a `GivenA[Thing]` builder. Builder methods name domain state — not raw setters.

```csharp
var match = new GivenAMatch()
    .WithHomeTeam(Team.England)
    .ThatIncludesAnInjury()
    .Build();
```

Rules:
- Name them `GivenA[Thing]` or `GivenAn[Thing]`
- Methods name domain state: `.ThatIsAnAdmin()`, `.ThatGoesToPenalties()`
- `Build()` always produces a valid default with no required configuration
- Start inline in the test suite; extract a builder when setup recurs

## What to test

Test behaviours — observable outcomes from the outside:
- Return values
- State changes visible through the public API
- Interactions with dependencies

Do not test private methods, internal data structures, or how the implementation achieves its result. If a private method feels like it needs its own test, it belongs on a separate class.

## Sequencing

Progress through test cases in this order:

1. **Simplest happy path** — minimal valid input producing expected output
2. **Further happy paths** — other valid inputs and normal variations
3. **Unhappy paths** — invalid inputs, failed preconditions, expected errors (e.g. duplicate email, missing resource)
4. **Edge cases** — boundary values (empty collections, zero, max values, single-item lists)
5. **Corner cases** — combinations of constraints that interact (e.g. max-length input that is also a duplicate)

Let complexity emerge from the tests — don't anticipate it. One test, one implementation step at a time. Complete red → green → refactor before moving to the next case.
