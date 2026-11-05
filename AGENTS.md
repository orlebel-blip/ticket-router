# Repository Guidelines

## Project Scope & Required Output

This project generates only Python code intended to run in Google Colab, including `.ipynb` notebooks containing Python cells. For code-generation requests, return Colab-ready Python code only; put any necessary explanations in Python comments. Do not generate other programming languages, standalone applications, or local-only setup instructions. Organize examples into cells that can be copied into Colab and executed from top to bottom.

## Ticket Router Notebook Task

Create a Google Colab notebook named `ticket_router.ipynb` with a simple LangGraph graph that routes customer support tickets. The state must hold the ticket, its category, and the reply. A `classify` node must use simple keyword rules to categorize each ticket and route it through a conditional edge to either a `billing` node or a `technical` node. Each destination node must populate the reply. Do not use an LLM or require an API key.

Include unexecuted Python cells that invoke the graph on one sample ticket for each routing path and draw the graph when the user runs the notebook. Generate the file only: do not execute the notebook, run tests, or commit it. This task-specific restriction overrides the general validation guidance below.

## Project Structure & Module Organization

This workspace currently contains this guide. Keep Python examples small and self-contained, with descriptive `snake_case.py` filenames. Avoid dependencies on local modules or files unavailable in a fresh Colab runtime. Include any required data upload or download steps in the example.

## Build, Test, and Development Commands

No local build, test, or lint commands are configured. Google Colab is the execution environment. Include dependency installation in an initial Colab cell, using `%pip install` when needed, followed by imports, configuration, and runnable example cells. Use `/content/` for temporary files and explicitly mount Google Drive only when required. Do not assume a GPU or persistent runtime state.

## Coding Style & Naming Conventions

Use four-space indentation, `snake_case` for modules and functions, and descriptive variable names. Keep functions focused and comments concise. No formatter or linter is configured.

## Testing Guidelines

No testing framework or coverage threshold is established. Validate examples by running all cells in order in a fresh Colab runtime when available. Include lightweight Python assertions for meaningful behavior. Keep checks deterministic where possible, and state when Colab execution has not been verified.

## Commit & Pull Request Guidelines

No Git history is available to establish a commit convention. Use short, imperative commit subjects, such as `Add basic graph example`, and keep changes focused. Pull requests should explain the purpose, list affected examples, and include validation commands and results. Link relevant issues when available; include screenshots only for visual changes.

## Security & Configuration

Never hardcode or print API keys or tokens. Use Colab Secrets through `google.colab.userdata.get` or a masked `getpass` prompt, and identify required secret names in Python comments.
