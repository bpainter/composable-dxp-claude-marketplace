---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[cloud-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Cloud Service Comparison Matrix (2025)

## Compute Services

| Use Case | AWS | Azure | GCP | Trade-offs |
|----------|-----|-------|-----|-----------|
| Long-running apps | EC2 | VMs | Compute Engine | Most control, highest management overhead |
| Containers | ECS/Fargate | ACI/AKS | Cloud Run | Abstraction vs. flexibility |
| Serverless functions | Lambda | Functions | Cloud Functions | Lowest ops, cold start, cost varies |
| Batch processing | Batch | Batch | Dataflow | Managed, cost-effective at scale |

## Database Services

| Requirement | AWS | Azure | GCP | Notes |
|-------------|-----|-------|-----|-------|
| Relational (SQL) | RDS | SQL Database | Cloud SQL | Managed, automated backups |
| NoSQL (key-value) | DynamoDB | Cosmos DB | Firestore | Low latency, auto-scaling |
| Analytics/DW | Redshift | Synapse | BigQuery | Excellent query performance, high cost for storage |
| Time-series | TimestreamDB | Data Explorer | Cloud TimeSeries | Optimized for time-series workloads |

## Storage & Data Transfer

| Service | Best For | Cost Pattern |
|---------|----------|-------------|
| S3/Blob/Cloud Storage | Object storage | Pay per GB stored + requests |
| EFS/Files/Filestore | Shared filesystem | Pay per GB-month |
| Glacier/Archive | Long-term backup | Cheap storage, expensive retrieval |
| CloudFront/CDN | Content delivery | Pay per GB egressed |

## 2025 Cloud Trends

1. **Multi-cloud adoption**: 82% of enterprises use 2+ clouds
2. **Serverless growth**: 65% of enterprises use serverless for at least some workloads
3. **FinOps focus**: Cost optimization is critical; dedicated FinOps teams are common
4. **Security-first**: Zero-trust architecture and compliance-as-code are standard
5. **AI/ML integration**: All clouds heavily pushing AI services (Bedrock, Vertex AI, OpenAI integration)
