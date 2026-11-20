# PDF RAG with LangChain

A small Retrieval-Augmented Generation (RAG) example that loads the bundled
`react_paper.pdf`, indexes its text in memory, and answers a hard-coded question
using OpenAI models and LangChain.

## How it works

When `main.py` runs, it:

1. Loads configuration from a `.env` file one directory above the project.
2. Parses `react_paper.pdf` with `UnstructuredLoader`.
3. Splits the extracted text into chunks of 1,000 characters with 100 characters
   of overlap.
4. Embeds the chunks and stores them in an `InMemoryVectorStore`.
5. Retrieves the three most relevant chunks and runs three examples:
   - a direct LLM call without retrieval;
   - a manually composed retrieval-and-generation function;
   - an LCEL retrieval chain.

The in-memory index is recreated and the PDF is re-embedded on every run. It is
not persisted between runs.

## Requirements

- Python 3.11 or newer
- [uv](https://docs.astral.sh/uv/)
- An OpenAI API key with access to the configured model (`gpt-6.1-sol`)

## Setup

Create a `.env` file in the directory immediately above this project directory.
At minimum, add:

```dotenv
OPENAI_API_KEY=your-openai-api-key
```

For LangSmith tracing, add your LangSmith key and enable tracing:

```dotenv
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your-langsmith-api-key
LANGSMITH_PROJECT=your-project-name
```

Keep `.env` private; do not commit API keys.

Install the project dependencies from the lock file:

```powershell
uv sync
```

## Run

From the project directory:

```powershell
uv run python main.py
```

The question is currently set in `main.py`. The script makes multiple model
calls, so running it requires a working API key, network access, and a model
available to your OpenAI account.

## Project files

- `main.py` — PDF loading, in-memory indexing, retrieval, and the three examples.
- `react_paper.pdf` — source document for the RAG examples.
- `pyproject.toml` and `uv.lock` — project dependencies and their locked versions.
