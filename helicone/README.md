# Helicone tutorial

> **Position:** Fastest integration, proxy-based

## Key differentiators

- **Change the proxy URL in one line** — no SDK required
- Supports 20+ providers including OpenAI, Anthropic, Gemini
- Cost tracking, request-level monitoring, caching
- Self-hostable (Docker)

## Prerequisites

1. Create a [Helicone](https://helicone.ai) account → obtain an API key
2. Obtain an API key from the [Anthropic Console](https://console.anthropic.com)
3. Create a `.env` file

```env
HELICONE_API_KEY=...
ANTHROPIC_API_KEY=sk-ant-...
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Quick start (OpenAI example)

```python
# Before
client = OpenAI(base_url="https://api.openai.com/v1")

# With Helicone (only this changes)
client = OpenAI(base_url="https://oai.helicone.ai/v1",
                default_headers={"Helicone-Auth": f"Bearer {HELICONE_API_KEY}"})
```

## Pricing

Free tier: 100,000 requests/month

## References

- [Helicone documentation](https://docs.helicone.ai)
