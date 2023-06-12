## AutoEvals

AutoEvals is a tool for quickly and easily evaluating AI model outputs. It comes with a variety of evaluation
methods, including heuristic (e.g. Levenshtein distance), statistical (e.g. BLEU), and model-based (using LLMs).

## Installation

To install AutoEvals, run the following command:

```bash
pip install autoevals
```

## Example

```python
from autoevals.llm import *

evaluator = Fact()
result = evaluator(
    output="People's Republic of China", expected="China",
    input="Which country has the highest population?"
)
print(result.score)
print(result.metadata)
```

## Supported Evaluation Methods

### Heuristic

- Levenshtein distance

- [ ] Jaccard distance
- [ ] BLEU

### Model-based Classification

- Battle
- ClosedQA
- Humor
- Factuality
- Security
- [ ] Summarization
- SQL
- Translation

### Other model based

- [ ] Embedding distance
- [ ] Fine-tuned classifiers

## Custom Evaluation Prompts

AutoEvals supports custom evaluation prompts. To use them, simply pass in a prompt and scoring mechanism:

```python
from autoevals import LLMClassifier

evaluator = LLMClassifier(
    """
You are a technical project manager who helps software engineers generate better titles for their GitHub issues.
You will look at the issue description, and pick which of two titles better describes it.

I'm going to provide you with the issue description, and two possible titles.

Issue Description: {{page_content}}

1: {{output}}
2: {{expected}}

Please discuss each title briefly (one line for pros, one for cons).
""",
    {"1": 1, "2": 0},
    use_cot=False,
)

page_content = """
As suggested by Nicolo, we should standardize the error responses coming from GoTrue, postgres, and realtime (and any other/future APIs) so that it's better DX when writing a client,

We can make this change on the servers themselves, but since postgrest and gotrue are fully/partially external may be harder to change, it might be an option to transform the errors within the client libraries/supabase-js, could be messy?

Nicolo also dropped this as a reference: http://spec.openapis.org/oas/v3.0.3#openapi-specification"""

gen_title = "Standardize error responses from GoTrue, Postgres, and Realtime APIs for better DX"
original_title = "Standardize Error Responses across APIs"


response = evaluator(gen_title, original_title, page_content=page_content)

print(f"Score: {response.score}")
```
