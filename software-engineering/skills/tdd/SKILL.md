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
RED      → Write a failing test for the next behaviour
GREEN    → Write minimum code to pass it
REFACTOR → Clean up with all tests green; no new behaviour
```

Cycles are short — minutes, not hours.

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

Start with the simplest case. Let complexity emerge from the tests — don't anticipate it. One test, one implementation step at a time.
