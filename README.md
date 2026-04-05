# AgentOps Learn

A hands-on learning project for comparing LLM observability / AgentOps tools.  
Each tool’s core features are covered step by step in Jupyter notebooks.

> For a broad market analysis, see [MARKET_REVIEW.md](./MARKET_REVIEW.md).

---

## Tutorials by tool

| Directory | Tool | Position | Status |
|-----------|------|----------|--------|
| [`langfuse/`](./langfuse/) | Langfuse | Leading open source, OTel-based, framework-agnostic | **Done** |
| [`agentops-ai/`](./agentops-ai/) | AgentOps.ai | Multi-agent focus, time-travel debugging | Planned |
| [`braintrust/`](./braintrust/) | Braintrust | Production eval + CI/CD deploy gates | Planned |
| [`helicone/`](./helicone/) | Helicone | Proxy-based, fastest integration | Planned |
| [`arize-phoenix/`](./arize-phoenix/) | Arize Phoenix | Fully open source, ML + LLM monitoring | Planned |

### Selection criteria

From the nine tools in MARKET_REVIEW.md, **five were chosen** for high hands-on value and clear positioning:

- LangSmith: tied to LangChain — excluded
- W&B Weave: tied to W&B — excluded
- Datadog: enterprise / paid focus — excluded
- Traceloop: instrumentation layer (not a backend) — excluded

---

## Shared environment setup

```bash
# From each tool directory
cd <tool-directory>

# Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run Jupyter
jupyter notebook
```

For API keys per tool, see each directory’s README.

---

## Feature comparison (summary)

| Capability | Langfuse | AgentOps | Braintrust | Helicone | Phoenix |
|------------|----------|----------|------------|----------|---------|
| Tracing | O | O | O | O | O |
| Multi-agent | Partial | **O** | O | X | O |
| Prompt management | O | X | O | X | X |
| LLM-as-a-Judge | O | X | O | X | O |
| CI/CD deploy gates | X | X | **O** | X | X |
| Time-travel debugging | X | **O** | X | X | X |
| Cost tracking | O | O | O | O | Partial |
| Fully open source | O (MIT) | X | X | Partial | O |
| Free self-hosting | O | X | X | O | O |

> For the full comparison, see [MARKET_REVIEW.md](./MARKET_REVIEW.md).
