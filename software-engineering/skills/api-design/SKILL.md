---
name: api-design
description: "Preferred patterns and conventions for building .NET/C# APIs with ASP.NET Core, MongoDB, Auth0, and xUnit. Apply proactively to all backend work."
---

# API Design (.NET/C#)

Preferred patterns and conventions for building .NET/C# APIs. Apply proactively to all backend work.

## Trigger conditions

Apply this skill when:
- Adding endpoints or designing domain models
- Setting up a new API project
- Writing tests or implementing repositories
- Integrating Auth0 or configuring CI/CD
- Making architecture decisions

## Stack

| Concern | Choice |
|---|---|
| Framework | ASP.NET Core — controllers, not minimal API |
| Language | C# |
| Database | MongoDB |
| Auth | Auth0 |
| Messaging | RabbitMQ (when needed) |
| Tests | xUnit + Testcontainers |

## Project structure

```
Solution
├── Api/        # Controllers, startup, DI
├── Domain/     # POCOs, Interfaces/, Services/
├── Storage/    # Repository implementations
├── External/   # Third-party providers
└── Tests/Api.Tests/
```

## Architecture

Start every feature as a vertical slice — Controller → Repository → Domain entity. Introduce MediatR **only when** a controller accumulates too many injected dependencies — not upfront.

## Domain

Plain C# classes — no EF, no MongoDB attributes, no framework references. Model the domain before touching infrastructure.

```csharp
public class User
{
    public string Id { get; private set; }
    public string AccountId { get; private set; }
    public string Email { get; private set; }

    public User(string id, string accountId, string email)
    {
        Id = id;
        AccountId = accountId;
        Email = email;
    }
}
```

## Repositories

One per aggregate root. Single `Save` method that upserts. Interface in `Domain/Interfaces/`, implementation in `Storage/Repositories/`.

```csharp
public interface IUserRepository
{
    Task Save(User user);
    Task<User?> GetById(string id);
    Task<IReadOnlyList<User>> GetByAccountId(string accountId);
}
```

## MongoDB persistence

Persist domain objects directly — no persistence DTOs. Use `ClassMapRegistrar` for private fields and polymorphic types. Register before any MongoDB operations.

## Auth

`ApiUser` unpacks Auth0 claims namespaced as `"{audience}/user_id"`, `"{audience}/account_id"`. `IApiUser` in domain contracts. Supports impersonation via `impersonate_account_id`. In tests: `FakeApiUser` — never wire JWT.

## Testing

Always start a new endpoint with an integration test asserting a success response.

```csharp
[Fact]
public async Task The_user_is_created_successfully()
{
    var response = await _client.PostAsJsonAsync("/users", new { Email = "john@example.com" });
    response.StatusCode.Should().Be(HttpStatusCode.Created);
}
```

Use Testcontainers for real infrastructure — don't fake the database at integration test level. Fakes not mocks. Extract `GivenA[Thing]` builders when setup recurs.

## CI/CD

GitHub Actions. Set up the test workflow immediately on project creation — never defer.

```yaml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '9.x'
      - run: dotnet test --no-build --verbosity normal
```

## Working style

Keep change sets small — one concern at a time. Always model the domain before touching infrastructure.
