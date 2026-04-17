---
name: ddd
description: "Domain-Driven Design principles: ubiquitous language, entities, value objects, aggregates, and domain services. Apply when designing or modelling a domain."
---

# Domain-Driven Design

The domain model is the heart of the software. All design decisions should reflect and reinforce the business domain.

## Trigger conditions

Apply this skill when:
- Designing or modelling a domain
- Creating entities, value objects, or aggregates
- Deciding where logic belongs
- Naming classes, methods, or concepts

## Ubiquitous language

Use domain terms everywhere — code, tests, conversations. If a domain expert would say "an invoice is approved", the code is `invoice.Approve()` — not `invoice.SetStatus(Status.Approved)`.

```csharp
// Good — speaks the domain
order.PlaceWith(supplier);
shipment.Dispatch();
payment.Authorise();

// Bad — technical noise
order.SetStatusToPlaced();
shipmentService.UpdateDispatchFlag(id, true);
```

If you find yourself writing comments to explain what code means, the names are wrong.

## Bounded contexts

Define explicit boundaries within which one model and one language apply. The same word can mean different things in different contexts — that is correct. Contexts communicate via interfaces, never by sharing internal models.

## Aggregates and aggregate roots

An aggregate is a cluster of objects treated as one unit for data changes. Every aggregate has a single root — the only entry point for the outside world.

- All access and mutation goes through the aggregate root — never directly to child entities
- The root enforces all invariants across the aggregate
- Keep aggregates small

```csharp
// Correct — through the root
var order = await _orderRepository.GetById(orderId);
order.AddLineItem(product, quantity);
await _orderRepository.Save(order);

// Wrong — bypassing the root
var lineItem = await _lineItemRepository.GetById(lineItemId); // ❌
lineItem.Quantity = 5; // ❌
```

## Entities vs value objects

**Entities** have identity — two entities with the same data are not the same thing.

**Value objects** are defined entirely by their attributes — immutable, interchangeable. Prefer value objects where possible.

```csharp
// Value object — immutable, defined by values
public record Money(decimal Amount, string Currency);
public record Address(string Line1, string City, string PostCode);
```

## Domain services

Use when an operation spans multiple aggregates. Stateless. No persistence, no HTTP. Lives in the domain layer. Use sparingly.

```csharp
public class TransferService
{
    public void Transfer(Account source, Account destination, Money amount)
    {
        source.Debit(amount);
        destination.Credit(amount);
    }
}
```

## Domain events

Past-tense facts raised by the aggregate root. Immutable value objects. Dispatched after save. The aggregate does not know who handles them.

```csharp
public record OrderPlaced(string OrderId, string CustomerId, DateTimeOffset PlacedAt);
```

## Interface dependencies

The domain defines what it needs via interfaces. It has no dependencies on infrastructure.

```
Domain   — defines IOrderRepository, IPaymentGateway
Storage  — implements IOrderRepository
External — implements IPaymentGateway
Api      — wires via DI
```

If the domain references a NuGet package outside the .NET standard library, a dependency has leaked inward.
