---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[cloud-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# High Availability & Disaster Recovery Patterns (2025)

## Availability Tiers

| SLA | Downtime/Year | Pattern | Complexity |
|-----|---|---------|-----------|
| 99% | 3.6 days | Single region, basic redundancy | Low |
| 99.9% | 8.7 hours | Multi-AZ, managed services | Medium |
| 99.99% | 52 minutes | Multi-region, active-active | High |
| 99.999% | 5 minutes | Multi-region, multiple providers | Very High |

## Multi-AZ (Within Region)

Simple, cost-effective for most applications.

```
Load Balancer
├── AZ-1 (EC2, RDS read replica)
└── AZ-2 (EC2, RDS primary)
```

**RTO**: 1-5 minutes (automated failover)
**RPO**: Near zero (synchronous replication)
**Cost**: +20-30% for redundancy

## Multi-Region (Active-Passive)

Standby region activates on primary failure.

```
Primary Region       Standby Region
└── App (active)     └── App (passive)
└── DB (active)      └── DB (read replica)

Global Load Balancer routes to primary, fails over to standby on health check failure
```

**RTO**: 5-15 minutes (DNS propagation + app startup)
**RPO**: Minutes (async replication)
**Cost**: Double infrastructure

## Multi-Region (Active-Active)

All regions serve traffic.

```
Region 1             Region 2             Region 3
└── App (active)     └── App (active)     └── App (active)
└── DB (replica)     └── DB (primary)     └── DB (replica)

Global Load Balancer distributes traffic, database handles replication
```

**RTO**: Seconds (automatic failover)
**RPO**: Seconds (async replication, tolerate conflicts)
**Cost**: Triple infrastructure, more complex

## Database Replication Strategies

### Synchronous (RPO ~0)
Primary waits for replica confirmation. Slower writes, guaranteed consistency.

```
Write Request
├── Write to primary
├── Wait for replica ACK
└── Confirm to client
```

### Asynchronous (RPO: seconds-minutes)
Primary confirms immediately, replicates in background. Fast writes, possible data loss.

```
Write Request
├── Write to primary
└── Confirm to client
  └── Replicate to standby (background)
```

### CRDT (Conflict-free Replicated Data Type)
Multiple primaries, automatic conflict resolution.

Best for: Distributed databases, strong availability needs
Example: DynamoDB with Global Tables

## Backup Strategy

```
RPO (Recovery Point Objective)
├── Continuous replication → RPO: minutes
├── Daily backups → RPO: 24 hours
└── Multi-region replication → RPO: seconds

RTO (Recovery Time Objective)
├── Automated failover → RTO: 1-5 minutes
├── Manual failover → RTO: 15-60 minutes
└── Cold restore from backup → RTO: hours
```

## Compliance Considerations

- **GDPR**: Data must stay in region (no US replication for EU data)
- **HIPAA**: Requires encryption, audit logs, minimum 2+ replicas
- **SOC 2**: Documented procedures, automated monitoring, incident response
