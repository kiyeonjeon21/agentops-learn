# Contributing

Thanks for your interest in contributing to AgentOps Learn!

## How to contribute

### Adding a new tool tutorial

Each tool lives in its own directory. To add tutorials for a planned tool (or propose a new one):

1. **Fork and clone** the repo
2. **Pick a directory** — `agentops-ai/`, `braintrust/`, `helicone/`, `arize-phoenix/`, or create a new one
3. **Follow the existing pattern** (see `langfuse/` as reference):
   - `README.md` — setup guide, key concepts, API reference
   - `requirements.txt` — tool-specific dependencies
   - Numbered notebooks (`01_`, `02_`, ...) — progressive tutorials
4. **Test your notebooks** — every cell should execute without errors
5. **Open a PR** with a clear description of what the tutorial covers

### Improving existing tutorials

- Fix typos, clarify explanations, update for SDK changes
- Add missing error handling or edge cases
- Improve code comments

### Updating the market review

- The market review lives in `docs/market-review.md`
- If pricing, features, or funding information has changed, PRs are welcome
- Please cite sources for factual claims

## Guidelines

- **Language:** All content should be in English
- **No secrets:** Never commit API keys or `.env` files
- **Keep it simple:** Tutorials should be beginner-friendly
- **Test before submitting:** Run all notebook cells end-to-end
- **One tool per PR:** Don't mix changes across multiple tool directories

## Development setup

```bash
# Clone
git clone https://github.com/kiyeonjeon21/agentops-learn.git
cd agentops-learn

# Set up a tool directory
cd langfuse/                    # or any tool directory
cp ../.env.example .env         # fill in your API keys
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

## Questions?

Open an issue if anything is unclear.
