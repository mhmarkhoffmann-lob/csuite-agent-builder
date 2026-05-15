# C-Suite Agent Builder — Claude Code Instructions

## Language

**All files in this repository must be written in English.**

This applies to:
- `agents/csuite-*.md` — subagent definitions
- `skills/csuite-*.md` — standalone skill definitions
- `README.md`, `CONTEXT.md`, `FOCUS.md`, docs

The language of conversation with the user may vary (German or English), but **file output is always English**, without exception.

## Repository Structure

- `agents/` — 12 subagent definitions, invoked by the Moderator via Agent tool
- `skills/` — 13 standalone skill definitions (12 roles + Moderator)
- Moderator orchestrates: Intake → Memory → Research → Board → Round 1 → Round 2 → Synthesis → Step 9 (optional counter-voice)

## Model

Model-agnostic. All agent and skill files inherit the model from the user's session — no hardcoded model values.
