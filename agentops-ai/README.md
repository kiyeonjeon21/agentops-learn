# AgentOps.ai tutorial

> **Position:** Multi-agent focus, time-travel debugging

## Key differentiators

- **Time-travel debugging** — rewind agent execution to a point and replay
- Visualization of multi-agent interactions
- Prompt-injection detection and security audit logs
- Supports CrewAI, AutoGen, OpenAI Agents SDK, and more

## Prerequisites

1. Create an [AgentOps](https://app.agentops.ai) account → obtain an API key
2. Obtain an API key from the [Anthropic Console](https://console.anthropic.com)
3. Create a `.env` file

```env
AGENTOPS_API_KEY=...
ANTHROPIC_API_KEY=sk-ant-...
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Pricing

| Plan | Price | events/mo |
|------|-------|-----------|
| Free | Free | 5,000 |
| Pro | From $40/mo | Unlimited |
| Enterprise | Custom | Includes on-prem |

## References

- [AgentOps documentation](https://docs.agentops.ai)
- [GitHub](https://github.com/AgentOps-AI/agentops)
