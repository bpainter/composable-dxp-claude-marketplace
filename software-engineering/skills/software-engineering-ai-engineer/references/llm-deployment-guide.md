---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[ai-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# LLM & Foundation Model Deployment (2025)

## Model Selection Strategies

### Fine-Tuning vs. Prompt Engineering vs. RAG

| Approach | Cost | Quality | Latency | Best For |
|----------|------|---------|---------|----------|
| Prompt Engineering | Low | Good | Fast | Task adaptation, few-shot learning |
| RAG | Medium | Excellent | Medium | Knowledge-grounded tasks |
| Fine-tuning (LoRA) | Medium | Very Good | Fast | Domain specialization, cost control |
| Full Fine-tuning | High | Excellent | Medium | High-stakes, custom behavior |

## Cost Optimization

### 1. Model Selection
- Smaller models (7B, 13B) for latency-sensitive tasks
- Distilled models (cheaper, faster)
- Quantized versions (4-bit, 8-bit)

### 2. Inference Optimization
```python
# Quantization reduces memory and latency
from transformers import AutoModelForCausalLM, BitsAndBytesConfig

config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.float16
)
model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-7b",
    quantization_config=config
)
```

### 3. Batching & Caching
- Batch inference requests
- Use KV-cache for generation
- Implement request deduplication

## Serving Architecture

### Option 1: Managed Services
- OpenAI API (simplest, highest cost per token)
- AWS Bedrock (managed, multi-model)
- Vertex AI (Google, strong integration)

### Option 2: Self-Hosted
- vLLM (fast inference)
- Ray Serve (scalable)
- TensorRT-LLM (NVIDIA, optimized)

```python
# Example: vLLM for fast inference
from vllm import LLM, SamplingParams

llm = LLM(model="meta-llama/Llama-2-7b-hf")
sampling_params = SamplingParams(temperature=0.8, top_p=0.95)

outputs = llm.generate(prompts, sampling_params)
```

## Monitoring & Safety

### Output Validation
- Toxicity filters
- Factuality checks (against knowledge base)
- Length/cost limits
- Guardrails for sensitive domains

### Evaluation Framework
- Human evaluation for quality
- Automated metrics (BLEU, ROUGE, F1)
- Cost per task tracking
- Latency percentiles (p50, p95, p99)

## 2025 Trends

- **Smaller, specialized models**: 7B-13B models fine-tuned often outperform larger models
- **Multimodal integration**: Vision + text for richer understanding
- **On-device inference**: Privacy and latency benefits for edge deployment
- **Cost awareness**: Customers increasingly care about inference cost
