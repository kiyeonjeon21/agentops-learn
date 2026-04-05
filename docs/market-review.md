# AgentOps market review

> **Date:** April 2026  
> **Scope:** The LLM observability / AgentOps landscape — key vendors, feature comparison, selection criteria

---

## 1. What is AgentOps?

**AgentOps** refers to the ecosystem of tools used to observe, evaluate, and debug LLM-based applications and AI agents in production.

Where traditional APM (Application Performance Monitoring) focuses on infrastructure metrics such as latency, error rates, and CPU, AgentOps focuses on metrics native to LLMs:

| Traditional APM | AgentOps |
|-----------------|----------|
| Response time, error rate | Token usage, API cost |
| CPU / memory | Prompt quality, hallucination |
| HTTP status codes | LLM output evaluation scores |
| Log search | Agent run replay |
| Single-function tracing | Multi-agent hierarchical tracing |

### Why AgentOps matters

- LLM outputs are **non-deterministic** — the same input can yield different results.
- Agents **dynamically combine** many LLM calls, tool calls, and sub-agents — failures are hard to localize.
- Without **data-driven verification**, prompt changes easily cause silent quality regressions.
- Token cost scales with usage — **finding waste** directly reduces spend.

---

## 2. Market landscape

### Size and growth

- Global GenAI model market in 2026: projected to exceed **$25B**.
- Gartner: LLM observability expected to reach **50% of GenAI deployments** by 2028 (vs. ~15% today).
- As of Q1 2026, **120+** agentic AI tools compete in the space.

### Notable funding

| Company | Round | Timing | Investors |
|---------|-------|--------|-----------|
| Braintrust | $80M (Series B) | Feb 2026 | ICONIQ Capital, a16z, Greylock |
| AgentOps.ai | $2.6M | 2024 | — |
| Langfuse | Not disclosed (open source) | — | — |

After its Series B, Braintrust reached a **$800M** valuation and positioned itself as an AI infrastructure layer.

---

## 3. Key vendors in detail

### 3-1. Langfuse

> **Position:** Leading open-source option; strong fit when data sovereignty matters

- **License:** MIT (fully open source)
- **GitHub:** ~12,000 stars (as of Feb 2026; among the fastest-growing LLM observability OSS projects)
- **SDK:** Python / JavaScript, OpenTelemetry-based (v3)
- **Self-hosting:** Free on Docker, no feature cap
- **Main features:** Tracing, prompt management, LLM-as-a-judge evals, dataset experiments

**Pricing (Cloud):**

| Plan | Price | Included |
|------|-------|----------|
| Hobby | Free | 50,000 units/mo, 30-day retention |
| Core | $29/mo | 100,000 units/mo |
| Pro | $199/mo | 3-year data retention |
| Enterprise | $2,499/mo | Unlimited |
| **Self-hosted** | **Free** | **No feature limits** |

**Best for:** Teams with governance/security requirements, teams wanting framework-agnostic tooling, teams preferring open-source communities.

---

### 3-2. LangSmith (LangChain)

> **Position:** Official observability for the LangChain / LangGraph ecosystem

- **License:** Proprietary (closed source)
- **Self-hosting:** Enterprise only (~$100K+/year)
- **Strengths:** Zero-config LangChain/LangGraph integration, strong eval workflows, human annotation
- **Weaknesses:** Much less value outside LangChain; per-trace pricing can be expensive

**Pricing:**

| Plan | Price | traces/mo |
|------|-------|-----------|
| Developer | Free | 5,000 |
| Plus | $39/seat/mo | 10,000 included |
| Enterprise | Custom | Unlimited + self-hosting |

**Performance overhead:** ~0% (minimal)

**Best for:** Teams that standardize on LangChain / LangGraph.

---

### 3-3. AgentOps.ai

> **Position:** Multi-agent focus, time-travel debugging

- **GitHub:** [AgentOps-AI/agentops](https://github.com/AgentOps-AI/agentops)
- **Frameworks:** CrewAI, AutoGen, OpenAI Agents SDK, 400+ LLMs
- **Differentiators:**
  - **Time-travel debugging** — rewind agent execution to a point and replay
  - Visualization of multi-agent interactions
  - Prompt-injection detection and security audit logs
  - LLM fine-tuning optimization (claims up to 25× cost reduction)

**Pricing:**

| Plan | Price | events/mo |
|------|-------|-----------|
| Free | Free | 5,000 |
| Pro | From $40/mo | Unlimited |
| Enterprise | Custom | Includes on-prem |

**Performance overhead:** ~12%

**Compliance:** SOC 2, HIPAA, NIST AI RMF

**Best for:** Teams running complex multi-agent systems where reproducible runs matter.

---

### 3-4. Braintrust

> **Position:** Production eval leader, CI/CD integration

- **Funding:** Series B $80M (Feb 2026, **$800M** valuation)
- **Customers:** Notion, Replit, Cloudflare, Ramp, Dropbox
- **Differentiators:**
  - **CI/CD deploy gates** — block production deploys when quality bars fail
  - Built-in AI assistant: analyze millions of traces → prompt suggestions + hallucination patterns
  - Brainstore (custom DB): claims ~80% faster complex AI trace queries
  - Statistical significance, hallucination detection

**Pricing:** 1M trace spans/mo free, then usage-based

**Best for:** Teams who want AI quality as a deploy gate, teams needing large-scale eval pipelines.

---

### 3-5. Helicone

> **Position:** Fastest integration, proxy-based

- **Self-hosting:** Free on Docker
- **Integration:** Change proxy URL in one line (no SDK required)
- **Providers:** OpenAI, Anthropic, Gemini, 20+ others
- **Features:** Cost tracking, request-level monitoring, caching
- **Weaknesses:** No deep eval features, limited multi-agent support

**Pricing:** Free 100,000 requests/mo

```python
# Before
client = OpenAI(base_url="https://api.openai.com/v1")

# With Helicone (only this changes)
client = OpenAI(base_url="https://oai.helicone.ai/v1",
                default_headers={"Helicone-Auth": f"Bearer {HELICONE_API_KEY}"})
```

**Best for:** Teams needing quick setup, teams focused on cost tracking.

---

### 3-6. Arize Phoenix

> **Position:** Fully open source, ML + LLM monitoring in one place

- **License:** Fully open source
- **Origin:** LLM capabilities spun out from ML monitoring vendor Arize AI
- **Foundation:** OpenTelemetry-native
- **Features:** Tracing, RAG quality evals, embedding visualization, drift detection
- **Weaknesses:** ML-first architecture can feel heavy for LLM-only teams

**Best for:** Teams monitoring both classic ML and LLMs.

---

### 3-7. Weights & Biases Weave

> **Position:** W&B’s LLM observability layer

- **Origin:** Extension of W&B (experiment tracking) into LLMs
- **Strengths:** Single platform for experiments + LLM observability for existing W&B users
- **OpenTelemetry:** OTLP via `/otel/v1/traces`
- **Weaknesses:** Fewer LLM-specific features vs. specialists; tied to W&B

**Best for:** Teams already on W&B for ML experiments.

---

### 3-8. Traceloop / OpenLLMetry

> **Position:** Vendor-neutral layer on OpenTelemetry

- **Core:** **OpenLLMetry** — OSS SDK extending OTel for LLMs
- **Traits:** Auto-instrumentation, swap backends (Langfuse, Datadog, Grafana, etc.)
- **Role:** **Instrumentation layer**, not a full observability backend

---

### 3-9. Datadog LLM Observability

> **Position:** Enterprise APM vendor extending into LLMs

- **Strengths:** LLM traces alongside servers, DBs, networks in **one platform**
- **Integration:** OpenTelemetry 1.37+ native
- **Weaknesses:** LLM-specific features (evals, prompt management) thinner than specialists; high cost

**Best for:** Large enterprises already on Datadog.

---

## 4. Feature comparison

| Capability | Langfuse | LangSmith | AgentOps | Braintrust | Helicone | Phoenix |
|------------|----------|-----------|----------|------------|----------|---------|
| **Tracing** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Multi-agent** | Partial | Partial | ✅ | ✅ | ❌ | ✅ |
| **Prompt management** | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |
| **LLM-as-a-Judge** | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |
| **Dataset experiments** | ✅ | ✅ | ❌ | ✅ | ❌ | Partial |
| **CI/CD deploy gates** | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Time-travel debugging** | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **Cost tracking** | ✅ | ✅ | ✅ | ✅ | ✅ | Partial |
| **Fully open source** | ✅ MIT | ❌ | ❌ | ❌ | Partial | ✅ |
| **Free self-hosting** | ✅ | ❌ (Enterprise) | ❌ | ❌ | ✅ | ✅ |
| **Performance overhead** | ~15% | ~0% | ~12% | N/A | Low | N/A |
| **Free tier** | 50K units | 5K traces | 5K events | 1M spans | 100K req | Unlimited |

---

## 5. Technical standard: OpenTelemetry

Since April 2024, OpenTelemetry (OTel) **GenAI semantic conventions** have become the de facto standard for LLM observability.

```
[LLM Application]
    ↓ OpenTelemetry SDK
[OTLP Exporter]
    ↓
┌───────────────────────────────────────────┐
│         Backend (your choice)             │
│  Langfuse / Datadog / Grafana / Honeycomb │
└───────────────────────────────────────────┘
```

**What standardization buys you:**

- **Less vendor lock-in** — swap backends without rewriting instrumentation
- **Simpler framework wiring** — CrewAI, AutoGen, LangChain, etc. largely support OTel
- **Langfuse v3 SDK** was rebuilt on OTel to align with this direction

---

## 6. How to choose

```
Must you own the data end-to-end?
├─ Yes → Langfuse (self-hosted) or Arize Phoenix
└─ No
    ├─ Is LangChain/LangGraph your main stack?
    │   └─ Yes → LangSmith
    ├─ Complex multi-agent system?
    │   └─ Yes → AgentOps.ai
    ├─ Need quality gates in CI/CD?
    │   └─ Yes → Braintrust
    ├─ Want setup in under 30 seconds?
    │   └─ Yes → Helicone
    └─ Already on W&B / Datadog?
        └─ Yes → Weave / Datadog LLM Observability
```

### By team size

| Team size | Recommendation | Rationale |
|-----------|----------------|-----------|
| Individual / side project | Langfuse Cloud (Hobby) | Free, capable |
| Early startup | Langfuse Cloud or Helicone | Fast setup, sensible cost |
| Growing startup | Braintrust or Langfuse Pro | Structured evals, CI/CD hooks |
| Enterprise (data-sensitive) | Langfuse self-hosted | Full control, cost-efficient |
| Enterprise (existing stack) | Datadog or LangSmith Enterprise | Integrate with incumbent tools |

---

## 7. Why Langfuse for this tutorial

Reasons Langfuse was picked here:

| Criterion | Assessment |
|-----------|------------|
| **Open source** | MIT, fully inspectable |
| **Framework-agnostic** | OTel-based; Anthropic / OpenAI / Gemini, etc. |
| **Feature breadth** | Tracing + prompts + evals + experiments in one place |
| **Learning curve** | Intuitive Python SDK, solid official cookbooks |
| **Self-hosting** | Docker one-liner, no artificial caps |
| **Momentum** | Among the fastest-growing LLM observability OSS projects |

**Where Langfuse is comparatively weaker:**

- Multi-agent hierarchy visualization → AgentOps.ai is stronger
- Automated CI/CD deploy blocking → Braintrust is stronger
- Performance overhead ~15% → higher than LangSmith (~0%)
- Depth of LangChain integration → LangSmith goes deeper

---

## 8. Summary

In 2026 the AgentOps market is maturing quickly. There is no single winner — **specialized tools coexist by use case.**

- **Open source + data sovereignty** → Langfuse
- **LangChain ecosystem** → LangSmith
- **Multi-agent debugging** → AgentOps.ai
- **Production eval + CI/CD** → Braintrust
- **Quick start + cost tracking** → Helicone
- **Technical standard** → OpenTelemetry GenAI semantic conventions

Whatever you pick, instrumenting on OTel minimizes code churn when you change backends later.
