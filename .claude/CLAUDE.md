# CLAUDE.md - Project Guidelines

## Project Memory (Mandatory)
- At the start of any session in a project, locate the project's memory directory at `~/.claude/projects/<project-slug>/memory/` and read `MEMORY.md` plus every linked memory file BEFORE the first non-trivial action.
- Treat project memories as binding instructions, not optional context. Apply user, feedback, project, and reference entries proactively - do not wait for them to be cited.
- If a memory conflicts with current repo state, verify the current state first, then update or remove the stale memory rather than acting on it.
- If the project directory has no memory folder yet, proceed normally but start building one as facts emerge (per the auto-memory rules in the harness).
- DO NOT add "Authored by Claude" comments when commiting code.

## How I Work (from usage insights)
- I hand you large, multi-part objectives and supervise in real-time. Run broadly, but stop the moment scope, target, or approach drifts from my mental model - do not wait until the end.
- I infer conventions from context (venv activation, file locations, variable reuse, naming). You should do the same: read the repo before asking.
- I favour centralisation, reusable workflows, composite actions, and the naming module. Prefer reusing existing resources, variables, NSGs, subnets, and configs over creating new ones - ask if unsure.
- I am tolerant of iteration on real progress, intolerant of wrong direction. Surface direction, scope, and target risks early, not after ten minutes of edits.
- I interrupt surgically rather than rewriting specs. When interrupted, stop immediately, reconcile against the correction, and re-confirm scope before continuing.

## Scope & Upfront Planning
- Before any multi-file edit: list every file you plan to touch with a one-line description of the change, flag every reuse-vs-create decision, and wait for my confirmation.
- Do not launch sub-agents for single-file investigations when I have named the file - read it directly.
- Avoid broad sed/regex replacements; prefer targeted edits and verify each match.
- For PR descriptions and PR reviews: confirm branch and template path first, output the draft only to chat, and never run `gh pr edit`, `gh pr create`, or write files until I explicitly approve.

## Domain-Specific API Discipline
- For specialised APIs (ai_forecast, alpaca-py, Terraform providers, Databricks Plugin Framework, GitHub Actions): verify current syntax against docs (WebFetch/WebSearch) BEFORE writing code, not after the first error.
- Pin GitHub Actions to the versions already used in the repo; check existing usage before upgrading.
- Never use dynamic blocks on Databricks Plugin Framework resources.
- After infrastructure edits, run `terraform fmt -recursive && terraform validate` and paste the output, flagging unexpected resource replacements or permission demotions before I approve.

## Core Rules
- Always read the relevant file(s) BEFORE attempting any Edit operations. Never edit a file you haven't first read in the current session.
- When fixing bugs in config files (especially YAML/JSON), first understand the full scope of the issue by reading ALL related config files before proposing changes. Ask clarifying questions about intent (e.g., 'Should I remove this entirely or just fix the duplicate?') rather than assuming a minimal fix.
- For multi-file refactors and standardization tasks, create a TODO list first using TodoWrite, then work through items sequentially. Confirm the pattern/ approach on the first file before applying across the codebase.
- Before making any changes, read all files related to this issue and summarize what you find. Then propose your approach and wait for my confirmation before editing anything.
- Run the linter on all Python files in this project. Fix every error you find. After each fix, re-run the linter on that file to confirm it passes before moving to the next.
- Never use the em dash - always use the hyphen instead.

## Planning & Solution Design

- **Explore before implementing**: When designing solutions or approaches, familiarise yourself with the existing codebase first. Use planning mode for complex tasks.
- **Track large efforts**: For multi-faceted solutions, create a markdown planning document (e.g., `PLAN.md`) to outline approach, track progress, and document decisions.
- **Ask clarifying questions**: If requirements are ambiguous or an approach could go multiple directions, ask before proceeding.

## Environment & Execution

- **Virtual environments**: Active virtual environments using the command below:
  ```bash
  activate_venv
  ```
- **Dependency management**: Check for `pyproject.toml`, `requirements.txt`, or `poetry.lock` to understand project dependencies before making changes.
- **uv projects**: If the project uses `uv`, activate the project venv before running any Python or test command. Try to activate the virtual envirionment with the shell command 'activate_venv' - if that fails, use 'mkvenv <Python Version>' to create a new virtual environment.
- **Python imports**: Ensure all necessary Python modules are imported at the beginning of your scripts.
- **Post-edit verification**: After editing Python files, run lint + mypy + tests and report results before declaring done. If there is a Makefile with `lint` and `format` targets, use those instead.

## Permissions & Automation

- **Web search**: Auto-approved for research, documentation lookup, and gathering context.
- **File operations**: Create, modify, and organise files as needed to complete tasks.

## Code Quality Standards

- **Match existing patterns**: Follow the conventions and style already established in the codebase.
- **Incremental changes**: Prefer smaller, testable changes over large rewrites unless explicitly requested.
- **Document decisions**: Add comments or documentation for non-obvious implementation choices.

## Communication

- **Show your reasoning**: Explain the "why" behind significant decisions, not just the "what".
- **Surface risks early**: If you identify potential issues, edge cases, or trade-offs, raise them proactively.
- **Summarise progress**: For longer tasks, provide brief status updates on what's been completed and what remains.

## Read-First Investigation Before Any Edits# 

Before making ANY edits, follow this exact process:

1. INVESTIGATE: Read all files related to the issue. Use Grep to find every reference to the affected code. Use Glob to discover related config files (especially YAML and JSON configs). Map the full dependency chain.

2. ANALYZE: Write a TodoWrite checklist documenting:
   - Every file that references the affected code
   - The root cause (not just the symptom)
   - All downstream effects of any change
   - Potential approaches ranked by risk

3. PROPOSE: Present your plan with the specific files and line ranges you'll modify. Wait for approval.

4. EXECUTE: Make changes file by file, verifying each change doesn't break related files.

5. VALIDATE: Re-read all modified files and their dependents to confirm consistency.

Now investigate this issue: [describe issue here]
