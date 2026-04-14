# Manual Test Case Generator

This project reads requirements from `.docx` or `.txt` files and generates manual test cases in a Word document (`.docx`).

It supports:
- default rule-based test case generation
- optional Ollama model-based logical scenario generation (`deepseek-r1` by default)
- execution logging and JSONL audit trail

## Features

- Creates both UI and API test cases
- Organizes output into separate `UI Test Cases` and `API Test Cases` sections
- Supports requirement input from `.docx` and `.txt`
- Uses Ollama (optional) to generate richer logical scenarios
- Falls back to default generation if Ollama fails or times out
- Writes run logs and audit events to files

## Setup

1. Create and activate a virtual environment (optional).
2. Install dependencies:

```bash
pip install -r requirements.txt
```

## Input

Default input file is `requirements_input.docx`.

Supported formats:
- `.docx` (reads non-empty paragraphs)
- `.txt` (reads non-empty lines)

Example requirement line:

```text
gmail account and select all the mail from mentioned receiver and delete all of them
```

## Usage

### Default generation

```bash
python generate_test_cases.py -i requirements_input.docx -o manual_test_cases.docx
```

### Ollama generation (DeepSeek-R1)

```bash
python generate_test_cases.py \
  --use-ollama \
  --ollama-model deepseek-r1 \
  --ollama-url http://localhost:11434 \
  --ollama-timeout 300 \
  -i requirements_input.docx \
  -o manual_test_cases.docx
```

If Ollama is unavailable, returns invalid JSON, or times out, the script automatically falls back to default generation.

## CLI Options

- `-i, --input` input requirements file (default: `requirements_input.docx`)
- `-o, --output` output `.docx` file (default: `manual_test_cases.docx`)
- `--use-ollama` enable Ollama-based logical scenario generation
- `--ollama-model` model name (default: `deepseek-r1`)
- `--ollama-url` Ollama server URL (default: `http://localhost:11434`)
- `--ollama-timeout` Ollama timeout in seconds (default: `120`)
- `--log-file` execution log file path (default: `logs/generator.log`)
- `--audit-file` audit JSONL file path (default: `logs/audit.jsonl`)
- `--verbose` enables verbose console logging

## Logging and Auditing

After each run:
- execution logs are written to `logs/generator.log`
- audit events are written to `logs/audit.jsonl`

Audit file includes events like:
- `run_started`
- `requirements_loaded`
- `ollama_fallback` (if applicable)
- `run_completed` (success/failure)

## Output

The generated file (`manual_test_cases.docx`) includes, for each test case:
- Test case ID (`UI-TC-xxx` / `API-TC-xxx`)
- Title
- Priority
- Preconditions
- Test steps
- Expected result
