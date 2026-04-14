# Manual Test Case Generator

This Python project reads requirements from a text file and creates manual test cases in a Word document (`.docx`).
For each requirement, it generates:
- one UI test case
- one API test case

## Setup

1. Create and activate a virtual environment (optional but recommended).
2. Install dependencies:

```bash
pip install -r requirements.txt
```

## Input Format

Put requirements in `requirements_input.docx` (or another input file you pass).  
Supported input formats: `.docx` and `.txt`.

Example:

Example requirement:

```text
gmail account and select all the mail from mentioned receiver and delete all of them
```

## Run

```bash
python generate_test_cases.py -i requirements_input.docx -o manual_test_cases.docx
```

## Output

The generated file (for example `manual_test_cases.docx`) contains:
- Separate sections for UI and API test cases
- Test case ID
- Title
- Priority
- Preconditions
- Test steps
- Expected result
