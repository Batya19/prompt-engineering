# Prompt Engineering – Natural Language → CLI

A Gradio app that turns a natural-language request into a single Linux Bash command
using GPT. The project focuses on **prompt engineering**: the system prompt was refined
in several iterations (earlier versions are kept as comments in `main.py`) until the
model reliably returned one valid command and refused dangerous ones.

## Prompt iterations

1. Return only a CLI command, no explanation, no markdown.
2. Lock the target to Linux Bash, forbid Windows / PowerShell / CMD output, and handle
   multi-step or ambiguous requests.
3. Add safety rules: destructive commands (deleting files, formatting disks, shutdown,
   system config changes) return exactly `REFUSED`.

The final prompt is the active `SYSTEM_PROMPT` in `main.py`. The model runs with
`temperature=0` for deterministic output.

## Files

- `main.py` – Gradio UI and the OpenAI call.
- `פרומפטים.pdf` – the prompt versions and the reasoning behind each change.
- `טבלת תרחישי בדיקות.pdf` – test-scenario table used to evaluate each prompt version.

## Setup

Requires Python 3.8+ and [uv](https://docs.astral.sh/uv/).

```bash
uv sync
```

Create a `.env` file:

```
OPENAI_API_KEY=your_key_here
```

## Run

```bash
uv run python main.py
```

Open the local Gradio URL printed in the terminal, type a request such as
*"Find all PDF files larger than 10MB"*, and the app returns the matching Bash command.
