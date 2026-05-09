---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[backend-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Service Decomposition & Microservices (2025)

## Monolith vs. Microservices Decision Matrix

| Factor | Monolith Wins | Microservices Wins |
|--------|---------------|-------------------|
| Team size | <20 engineers | >50 engineers |
| Deployment frequency | Weekly or less | Multiple per day |
| Independent scaling | Rare | Frequent |
| Data consistency | Strongly consistent | Eventual consistency OK |
| Operational complexity | Low | High |
| Initial time-to-market | Fast | Slower |

## When to Extract a Microservice from Monolith

Extract when:
1. **Independent scaling**: One component needs different resources
2. **Independent deployment**: Team needs to release without coordinating
3. **Clear boundaries**: Domain-driven design suggests separate bounded context
4. **Technology heterogeneity**: Different tech stack needed for this component
5. **Organizational alignment**: Conway's Law—org structure mirrors system

Don't extract just because it's "modern."

## Service Boundary Design

Use domain-driven design:

```
Order Service
├── Owns: order creation, fulfillment, history
├── Owns: order database
└── API: createOrder, getOrder, cancelOrder, etc.

Payment Service
├── Owns: payment processing, refunds
├── Owns: transaction ledger
└── API: processPayment, refund, transactionStatus

Inventory Service
├── Owns: stock levels, reservations
├── Owns: inventory database
└── API: reserveItem, releaseReservation, updateStock
```

Each service has single responsibility and owns its data.

## Data Consistency Patterns

### Immediate Consistency (Traditional Transactions)
Works when data lives in one database. Not applicable to microservices.

### Eventual Consistency (Event-Driven)
Services publish events; others react asynchronously.

```
Order Service publishes: OrderCreated
  └── Inventory Service subscribes: Reserve stock
  └── Payment Service subscribes: Process payment
  └── Notification Service subscribes: Send confirmation
```

### Saga Pattern (Distributed Transactions)
Coordinator orchestrates multi-service transaction, with compensating actions for rollback.

```
Booking Saga
├── Step 1: Reserve hotel
├── Step 2: Book flight (if hotel succeeds)
├── Step 3: Charge payment (if flight succeeds)
├── Rollback: Cancel hotel & flight if payment fails
```

## Anti-Patterns to Avoid

1. **Database sharing**: Services sharing database breaks encapsulation
2. **Synchronous chains**: Service A → Service B → Service C (cascading failures)
3. **No API contracts**: Tight coupling through undocumented APIs
4. **Shared libraries for logic**: Duplicate code or version hell
5. **Over-decomposition**: Too many tiny services (operational overhead)

## 2025 Trend: Modular Monolith

Monolith with clear module boundaries, independently deployable via feature flags, avoiding microservices overhead while maintaining architectural discipline.

```
Single codebase, clear module structure:
├── modules/
│   ├── orders/
│   ├── payments/
│   ├── inventory/
│   └── notifications/
├── Clear APIs between modules
├── Modules can be extracted to microservice when needed
└── Deploy entire monolith, but feature-flag module behavior
```

Best of both worlds: simplicity + scalability option.
