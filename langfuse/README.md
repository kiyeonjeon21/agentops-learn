# Langfuse Tutorial

A hands-on LLM Observability project using Langfuse + Claude (Anthropic).
Five notebooks cover basic tracing through prompt management, evaluation, and experiments step by step.

---

## Environment setup

### Prerequisites

1. Create a [Langfuse Cloud](https://cloud.langfuse.com) account → set up a project → obtain API keys
2. Obtain an API key from the [Anthropic Console](https://console.anthropic.com)
3. Create a `.env` file in this directory (`langfuse/`), or copy the root `.env` if it already exists:

```bash
# If you already have a .env in the project root:
cp ../.env .env
```

```env
LANGFUSE_SECRET_KEY=sk-lf-...
LANGFUSE_PUBLIC_KEY=pk-lf-...
LANGFUSE_BASE_URL=https://cloud.langfuse.com
CLAUDE_API_KEY=sk-ant-...
```

### Installation

```bash
# Run from the langfuse directory
cd langfuse/

# Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Register as a Jupyter kernel (for kernel selection in IDEs)
python -m ipykernel install --user --name langfuse-learn --display-name "langfuse-learn"

# Launch Jupyter
jupyter notebook
```

### Key packages

| Package | Role |
|---------|------|
| `langfuse` | Langfuse Python SDK v3 (OTel-based) |
| `anthropic` | Claude API client |
| `opentelemetry-instrumentation-anthropic` | Automatic Anthropic tracing |
| `python-dotenv` | Load `.env` environment variables |

---

## Tutorial structure

```
langfuse/
├── .env                                # API keys (create manually)
├── requirements.txt
├── 01_basic_tracing.ipynb              # Langfuse connection + auto-tracing
├── 02_decorator_and_nesting.ipynb      # @observe() + RAG pipeline
├── 03_prompt_management.ipynb          # Prompt creation / versioning
├── 04_scoring_and_evaluation.ipynb     # Scoring + LLM-as-a-Judge
└── 05_datasets_and_experiments.ipynb   # Datasets + A/B experiments
```

---

## 01. Basic Tracing (`01_basic_tracing.ipynb`)

**Key concept:** Once you set up `AnthropicInstrumentor`, all subsequent Claude API calls are automatically recorded in Langfuse.

**Topics covered:**
- `get_client()` / `auth_check()` — verify connection
- `AnthropicInstrumentor().instrument()` — enable auto-tracing
- Standard calls, streaming calls, multi-turn conversation tracing

**Core code:**
```python
from langfuse import get_client
from opentelemetry.instrumentation.anthropic import AnthropicInstrumentor

AnthropicInstrumentor().instrument()   # All Anthropic calls are now auto-traced
langfuse = get_client()
```

**Sample output:**
```
Langfuse auth successful! Connected to the dashboard.
Anthropic auto-tracing enabled

# 3 Reasons LLM Observability Matters
## 1. Quality & Safety — LLM outputs are non-deterministic; same input can yield different results...
## 2. Cost Optimization — Track token usage and API call counts to cut unnecessary costs...
## 3. Continuous Improvement — Improve prompts based on real user input/output data...

# Multi-turn conversation
[Message 1] Hello Alex! Nice to meet you!
[Message 2] Yes, I remember! You're Alex!
```

---

## 02. Decorators & Nested Traces (`02_decorator_and_nesting.ipynb`)

**Key concept:** Nesting `@observe()` decorators mirrors the function call hierarchy directly in Langfuse traces.

**Topics covered:**
- `@observe()` — auto-capture function I/O, execution time, errors
- Nested traces — RAG pipeline simulation
- `propagate_attributes()` — attach user_id, session_id, tags, metadata
- Automatic error capture

**Trace hierarchy:**
```
rag_pipeline              ← Top-level Trace
  ├─ retrieve_context     ← Span
  ├─ build_prompt         ← Span
  └─ generate_answer      ← Span
       └─ [Claude call]   ← Generation (tokens, cost auto-recorded)
```

**Core code:**
```python
from langfuse import observe, propagate_attributes

@observe()
def rag_pipeline(query: str) -> str:
    with propagate_attributes(user_id="user_42", tags=["rag"]):
        context = retrieve_context(query)   # nested @observe()
        return generate_answer(query, context)  # nested @observe()
```

**Sample output:**
```
Question: What is Langfuse?
Answer: Langfuse is an open-source observability platform for LLM applications.
        It provides tracing, prompt management, and evaluation features,
        and its Python SDK is based on OpenTelemetry.

[Error case] Query is empty  ← Errors are also auto-recorded in the trace
```

---

## 03. Prompt Management (`03_prompt_management.ipynb`)

**Key concept:** Manage prompts centrally in Langfuse instead of hardcoding them. Changes take effect immediately without deployment, and version history is preserved.

**Topics covered:**
- `create_prompt()` — create Text / Chat type prompts
- `get_prompt()` — fetch the latest version by production label
- `compile(**kwargs)` — substitute `{{variable}}` template variables
- Version updates and retrieving specific versions

**Core code:**
```python
# Create
langfuse.create_prompt(
    name="text-translator",
    prompt="Please translate the following text into {{target_language}}.\n\nOriginal:\n{{source_text}}",
    config={"model": "claude-sonnet-4-6", "temperature": 0.3},
    labels=["production"]
)

# Retrieve and use
prompt = langfuse.get_prompt("text-translator")
compiled = prompt.compile(target_language="English", source_text="Hello")
```

**Sample output:**
```
'text-translator' prompt created  (v1, temperature: 0.3)
'code-reviewer' prompt created

Source (to English): Hello, the weather is really nice today!
Translation: Hello, the weather is really nice today!

Source (to Korean): The quick brown fox jumps over the lazy dog.
Translation: ...

# Code review result (SQL injection detected)
Critical issue: f-string query construction → SQL injection vulnerability
Fix: Use parameter binding (cursor.execute(query, (user_id,)))

'text-translator' v2 created (temperature lowered to 0.2)
v1 temperature: 0.3  →  Latest production temperature: 0.2
```

---

## 04. Scoring & Evaluation (`04_scoring_and_evaluation.ipynb`)

**Key concept:** Measure LLM output quality numerically. Covers both manual scoring and LLM-as-a-Judge using Claude to evaluate other responses.

**Topics covered:**
- `create_score()` — add manual scores to traces
- NUMERIC / CATEGORICAL / BOOLEAN score types
- LLM-as-a-Judge — Claude evaluates Claude's output

**Core code:**
```python
# Manual score
langfuse.create_score(
    trace_id=trace_id,
    name="helpfulness",
    value=0.95,
    data_type="NUMERIC",
    comment="Clear and accurate answer"
)

# LLM-as-a-Judge: 4 metrics — accuracy, completeness, clarity, overall
```

**Sample output:**
```
# Q1: Capital of South Korea?  → Trace ID: 8ac6f1ff...
# Q2: List vs tuple?           → Trace ID: aa56a1fd...
# Q3: HTTP vs HTTPS?           → Trace ID: f85f52e3...

# LLM-as-a-Judge result (ML vs deep learning question)
{
  "accuracy": 8,        # Core relationship explained correctly
  "completeness": 4,    # Key differences missing (feature extraction, data requirements, etc.)
  "clarity": 7,
  "overall": 6,
  "reasoning": "Short and concise but missing important information for a complete answer"
}

# GIL question: overall 7/10
# REST vs GraphQL question: overall 9/10
```

---

## 05. Datasets & Experiments (`05_datasets_and_experiments.ipynb`)

**Key concept:** Run multiple task versions against the same test cases (dataset) for A/B comparison.

**Topics covered:**
- `create_dataset()` / `create_dataset_item()` — build benchmark datasets
- `run_experiment()` — automate experiments with task + evaluator functions
- keyword evaluator vs LLM-as-a-Judge evaluator comparison

**Dataset:** `korean-qa-benchmark` (5 items)

| # | Question | Category |
|---|----------|----------|
| 1 | Capital of South Korea? | geography |
| 2 | List vs tuple difference? | programming |
| 3 | HTTP 404 meaning? | web |
| 4 | O(n log n) algorithms? | algorithms |
| 5 | INNER JOIN vs LEFT JOIN? | database |

**Core code:**
```python
from langfuse.experiment import Evaluation

# Evaluator signature: receives input, output, expected_output, metadata as individual args
def keyword_evaluator(*, input, output, expected_output=None, metadata=None, **kwargs) -> Evaluation:
    score = compute_keyword_overlap(output, expected_output)
    return Evaluation(name="keyword_match", value=score)

result = langfuse.run_experiment(
    name="simple-qa-v1",           # not experiment_name
    run_name="claude-sonnet-no-system-prompt",
    data=dataset_items,            # not dataset_name — pass the actual list
    task=simple_qa_task,
    evaluators=[keyword_evaluator],
)
```

**Experiment results (with LLM-as-a-Judge):**
```
# Experiment: simple-qa-llm-judge

Item 1: Capital of South Korea?
  keyword_match:   0.500
  llm_judge_score: 1.000  ← "Correctly and clearly answered Seoul"

Item 2: List vs tuple?
  keyword_match:   0.250
  llm_judge_score: 0.900  ← "Key differences accurately explained with code examples"

Item 3: HTTP 404 meaning?
  keyword_match:   0.667
  llm_judge_score: 1.000  ← "Accurate and complete explanation"
```

> **Why keyword_match scores are low:** Simple word matching can be inaccurate due to paraphrasing and synonyms. LLM-as-a-Judge scores better reflect actual quality.

---

## Test results summary

| Notebook | Result | Notes |
|----------|--------|-------|
| 01_basic_tracing | Passed | |
| 02_decorator_and_nesting | Passed | |
| 03_prompt_management | Passed | |
| 04_scoring_and_evaluation | Passed | |
| 05_datasets_and_experiments | Passed (after fix) | SDK API changes applied |

### SDK API changes (latest langfuse version)

`run_experiment()` parameters have changed:

```python
# Before (old version)
langfuse.run_experiment(
    experiment_name="...",
    dataset_name="...",
    ...
)

# After (current)
langfuse.run_experiment(
    name="...",            # experiment_name → name
    data=dataset_items,    # dataset_name → actual list or DatasetItem list
    ...
)
```

Evaluator function signature also changed:

```python
# Before (old version)
def my_evaluator(*, item, output, **kwargs) -> dict:
    return {"score_name": 0.9}

# After (current)
from langfuse.experiment import Evaluation

def my_evaluator(*, input, output, expected_output=None, metadata=None, **kwargs) -> Evaluation:
    return Evaluation(name="score_name", value=0.9, comment="explanation")
```

---

## API reference

| API | Description |
|-----|-------------|
| `get_client()` | Initialize Langfuse client from environment variables |
| `langfuse.auth_check()` | Verify server connection |
| `langfuse.flush()` | Immediately send buffered data (required before script exit) |
| `AnthropicInstrumentor().instrument()` | Enable automatic Anthropic SDK tracing |
| `@observe()` | Apply tracing decorator to a function |
| `propagate_attributes(user_id, session_id, tags, metadata)` | Attach context attributes to traces |
| `create_prompt(name, prompt, config, labels)` | Create or update a prompt |
| `get_prompt(name, version=N, label="production")` | Retrieve a prompt |
| `prompt.compile(**vars)` | Substitute template variables |
| `create_score(trace_id, name, value, data_type)` | Add a score to a trace |
| `create_dataset(name)` | Create a dataset |
| `create_dataset_item(dataset_name, input, expected_output)` | Add a dataset item |
| `run_experiment(name, data, task, evaluators)` | Run an experiment |

---

## References

- [Langfuse documentation](https://langfuse.com/docs)
- [Langfuse Anthropic integration guide](https://langfuse.com/guides/cookbook/integration_anthropic)
- [Langfuse Python SDK v3](https://langfuse.com/docs/sdk/python/example)
- [Anthropic model list](https://docs.anthropic.com/en/docs/about-claude/models/all-models)
