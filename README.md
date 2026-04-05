# AgentOps Learn

Hands-on tutorials for LLM observability and AgentOps tools.  
Each directory is a self-contained module with its own notebooks, dependencies, and setup instructions.

---

## Project structure

```
agentops-learn/
├── docs/
│   └── market-review.md            # Market analysis — 9 vendors compared
│
├── langfuse/                       # ✅ Done
│   ├── 01_basic_tracing.ipynb
│   ├── 02_decorator_and_nesting.ipynb
│   ├── 03_prompt_management.ipynb
│   ├── 04_scoring_and_evaluation.ipynb
│   └── 05_datasets_and_experiments.ipynb
│
├── agentops-ai/                    # 📋 Planned
├── braintrust/                     # 📋 Planned
├── helicone/                       # 📋 Planned
├── arize-phoenix/                  # 📋 Planned
│
├── .env.example                    # All API keys in one place
└── .gitignore
```

## Tutorials

| Directory | Tool | What it covers | Status |
|-----------|------|----------------|--------|
| [`langfuse/`](./langfuse/) | Langfuse | Tracing, `@observe()`, prompt management, scoring, dataset experiments | **Done** |
| [`agentops-ai/`](./agentops-ai/) | AgentOps.ai | Multi-agent tracing, time-travel debugging | Planned |
| [`braintrust/`](./braintrust/) | Braintrust | Production evals, CI/CD deploy gates | Planned |
| [`helicone/`](./helicone/) | Helicone | Proxy-based integration, cost tracking | Planned |
| [`arize-phoenix/`](./arize-phoenix/) | Arize Phoenix | OTel-native tracing, embedding visualization | Planned |

> **Why these five?** Selected from [9 vendors reviewed](./docs/market-review.md) for high hands-on value and distinct positioning. Excluded: LangSmith (LangChain-tied), W&B Weave (W&B-tied), Datadog (enterprise), Traceloop (instrumentation layer only).

## Quick start

```bash
# 1. Copy and fill in your API keys
cp .env.example .env

# 2. Pick a tool directory
cd langfuse/               # (or agentops-ai/, braintrust/, etc.)

# 3. Copy .env and set up
cp ../.env .env
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 4. Run
jupyter notebook
```

## Further reading

- [Market review](./docs/market-review.md) — vendor comparison, pricing, selection criteria
- [OpenTelemetry GenAI Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) — the emerging standard all tools converge on
