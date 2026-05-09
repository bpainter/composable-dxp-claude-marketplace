---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[agentic-workflow-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Agent Frameworks Comparison (2025)

## Framework Landscape

### OpenAI Agents SDK (Recommended for New Projects)
- **Strengths**: Production-ready, state machine model, function calling, first-class tool support
- **Use Case**: Structured workflows with clear state transitions
- **Maturity**: Released March 2025, actively maintained
- **Best For**: Teams already using OpenAI APIs

### LangGraph (Graph-Based Orchestration)
- **Strengths**: Conditional routing, parallel execution, persistent state, debugging tools
- **Use Case**: Complex workflows requiring branching and loops
- **Maturity**: Stable, widely adopted
- **Best For**: Complex decision trees and RAG workflows

### Microsoft AutoGen (Multi-Agent at Scale)
- **Strengths**: Enterprise-grade multi-agent communication, conversation management
- **Version**: 0.4 (redesigned January 2025)
- **Use Case**: Multi-agent teams with specialization
- **Best For**: Large organizations needing governance

### LlamaIndex (Orchestration + Retrieval)
- **Strengths**: RAG-native, agent retrieval pipelines, data index management
- **Use Case**: Knowledge-grounded agents
- **Best For**: Document Q&A, knowledge base applications

## Framework Selection Matrix

| Requirement | Agents SDK | LangGraph | AutoGen | LlamaIndex |
|-----------|-----------|-----------|---------|-----------|
| Simplicity | ★★★★★ | ★★★★ | ★★★ | ★★★★ |
| Multi-Agent | ★★ | ★★★ | ★★★★★ | ★★ |
| Complex Routing | ★★★ | ★★★★★ | ★★★★ | ★★★ |
| RAG Integration | ★★ | ★★★★ | ★★★ | ★★★★★ |
| Production Ready | ★★★★★ | ★★★★ | ★★★★ | ★★★★ |

## 2025 Market Insights

- **61% of organizations** have begun agentic AI development (Gartner)
- **80% of effort** on agentic systems goes to data engineering and workflow integration
- **33% of enterprise software** will have agentic capabilities by 2028
- **Adoption rate**: 40% of agentic deployments will be canceled by 2027 due to poor ROI or cost—focus on validated use cases
