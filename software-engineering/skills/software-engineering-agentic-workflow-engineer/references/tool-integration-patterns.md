---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[agentic-workflow-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Tool Integration Patterns for Agents

## Tool Design Principles

### 1. Schema Clarity
Every tool must have explicit inputs, outputs, and error types.

```python
{
  "name": "fetch_customer_data",
  "description": "Retrieve customer information from CRM",
  "input_schema": {
    "type": "object",
    "properties": {
      "customer_id": {
        "type": "string",
        "description": "Unique customer identifier"
      }
    },
    "required": ["customer_id"]
  },
  "output_schema": {
    "type": "object",
    "properties": {
      "customer": {
        "type": "object",
        "description": "Full customer record"
      }
    }
  },
  "errors": ["CustomerNotFound", "AuthenticationError", "RateLimitExceeded"]
}
```

### 2. Error Handling Strategy
Tools must fail gracefully with structured error responses.

**Pattern**: Return error details with recovery hints
```
{
  "success": false,
  "error": "RateLimitExceeded",
  "message": "Rate limit reached. Retry after 30 seconds.",
  "recoverable": true,
  "retry_after_seconds": 30
}
```

### 3. Idempotency
Design tools to be safe for replay and retry.

- Use unique request IDs (idempotency keys)
- Implement deduplication at storage layer
- Log all invocations with timestamps

### 4. Testing & Validation
Mock tools for agent testing without hitting live APIs.

```python
# Mock tool for testing
def mock_fetch_customer(customer_id):
  if customer_id == "test-123":
    return {"customer": {"name": "Test User"}}
  return {"success": false, "error": "CustomerNotFound"}
```

## Common Tool Categories

### Information Retrieval
- Databases (SQL, NoSQL)
- Search engines (Elasticsearch, Algolia)
- APIs (REST, GraphQL)
- Knowledge bases

### Execution Tools
- File operations
- Email sending
- Calendar/scheduling
- External service calls

### Data Transformation
- Format conversion
- Aggregation
- Enrichment (joining external data)
- Validation

### Monitoring Tools
- Alerting
- Logging to observability platform
- Metric publishing

## Credential Management

Store credentials securely outside agent code:

```python
from vault import SecureVault

vault = SecureVault()
api_key = vault.get("stripe_api_key")
db_password = vault.get("postgres_password")
```

Use scoped credentials for least-privilege access.
