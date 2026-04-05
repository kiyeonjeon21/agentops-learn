# Arize Phoenix tutorial

> **Position:** Fully open source, ML + LLM monitoring in one place

## Key differentiators

- **Fully open source** (spun out from ML monitoring vendor Arize AI)
- OpenTelemetry-native
- Tracing, RAG quality evaluation, embedding visualization, drift detection
- Monitor classic ML models and LLMs together

## Prerequisites

1. Phoenix runs locally (no separate cloud account required)
2. Obtain an API key from the [Anthropic Console](https://console.anthropic.com)
3. Create a `.env` file

```env
ANTHROPIC_API_KEY=sk-ant-...
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Pricing

Free (open source, local execution)

## References

- [Phoenix documentation](https://docs.arize.com/phoenix)
- [GitHub](https://github.com/Arize-ai/phoenix)
