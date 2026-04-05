# Braintrust tutorial

> **Position:** Production eval leader, CI/CD deploy gates

## Key differentiators

- **CI/CD deploy gates** — block production deploys when quality bars fail
- Built-in AI assistant: trace analysis → prompt improvement suggestions + hallucination pattern detection
- Statistical significance checks, hallucination detection
- Customers: Notion, Replit, Cloudflare, Ramp, Dropbox

## Prerequisites

1. Create a [Braintrust](https://www.braintrust.dev) account → obtain an API key
2. Obtain an API key from the [Anthropic Console](https://console.anthropic.com)
3. Create a `.env` file

```env
BRAINTRUST_API_KEY=...
ANTHROPIC_API_KEY=sk-ant-...
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Pricing

1M trace spans/month free, then usage-based

## References

- [Braintrust documentation](https://www.braintrust.dev/docs)
