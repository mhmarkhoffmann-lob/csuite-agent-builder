# C-Suite Agent Builder

A modular "Board of Directors" as a Claude Code skill system. Instead of a generic assistant, you get an adaptively assembled board of C-suite perspectives that examines your decision across two discussion rounds plus a synthesis.

## What it does

`/csuite [topic]` starts a board session:

1. **Intake** — Moderator clarifies the decision, role, and context
2. **Research** — Autonomous research agent determines knowledge dimensions, gathers facts, delivers a Session Context Package
3. **Board Selection** — Moderator picks 2–4 fitting agents from the pool
4. **Round 1** — Each agent gives an independent assessment (parallel subagent calls)
5. **Round 2** — Agents respond to each other: direct contradiction, topic shifts, asymmetries
6. **Synthesis** — Consensus, tensions, recommendation

Individual agents are also usable standalone (`/csuite-cfo`, `/csuite-research`, etc.).

## Agent Pool

- **CEO** — Overall strategy, vision, long-term value
- **CFO** — Finance, ROI, risk
- **COO** — Operations, feasibility, resources
- **CHRO** — Career, leadership, culture
- **CIO** — IT architecture, data, scalability
- **CAIO** — AI-first perspective, disruption, automation
- **CSUO** — Sustainability, ESG, ecological and social impact
- **Chief Strategist** — Long-term strategy, mega-trends (10-year horizon)
- **Investor** — External contrarian view, kill scenarios
- **Ethicist** — Ethical implications, stakeholders, unintended consequences
- **Coach** — Personal values, clarity, authenticity

Plus two orchestrators:

- **Moderator** (`csuite-moderator.md`) — Adaptive board selector and discussion facilitator
- **Research** (`csuite-research.md`) — Autonomous pre-session research agent

## Example Constellations

- Career decision → CHRO + Coach + Chief Strategist
- Evaluate a business project → CFO + Investor + CEO
- Tech investment → CIO + CAIO + CFO
- Personal priorities → Coach + Ethicist + Chief Strategist
- Sustainability question → CSUO + Ethicist + CFO

## Installation

Copy skills to `~/.claude/skills/csuite-[name]/` (or package as a plugin for a marketplace). All skills are pure Markdown files with frontmatter — no build step required.

```bash
git clone https://github.com/mhmarkhoffmann-lob/csuite-agent-builder.git
cd csuite-agent-builder
for f in skills/csuite-*.md; do
  name=$(basename "$f" .md)
  mkdir -p ~/.claude/skills/"$name"
  cp "$f" ~/.claude/skills/"$name"/SKILL.md
done
```

## Using Without Claude Code

No Claude Code? No problem. The skill files are plain text prompts — you can run a full board session in any chat interface (ChatGPT, Claude.ai, OpenWebUI, Copilot, etc.) using the 3-step workflow below.

### Step 1 — Start with the Moderator

Open a new chat. Paste the full contents of `skills/csuite-moderator.md` as your **system prompt** (custom instructions / personality / system message — whatever your tool calls it).

Then describe your topic:

```
I need a board session on: [your decision or question]
```

The Moderator will ask a few clarifying questions and then tell you which 2–4 agents it wants to convene. Note those names — you'll need them in Step 2.

### Step 2 — Run each selected agent

For each agent the Moderator selected, open a **new chat**. Paste that agent's skill file as the system prompt (e.g. `skills/csuite-cfo.md` for the CFO).

Then give it the context from Step 1:

```
Topic: [your decision]
Context: [paste the clarifying answers you gave the Moderator]

Please give your independent assessment.
```

Copy the full response. Repeat for each selected agent.

### Step 3 — Synthesize

Go back to your Moderator chat from Step 1. Paste all agent responses in one message:

```
Here are the board assessments:

**CFO:** [paste response]

**Investor:** [paste response]

**CEO:** [paste response]

Please run Round 2 (agents respond to each other) and then deliver the synthesis and recommendation.
```

The Moderator generates the discussion and final recommendation.

---

**Tips**
- Use the strongest model available for Step 1 and Step 3 (Moderator). Agent steps in Step 2 can run on a lighter model.
- You can skip Step 2 entirely and ask the Moderator to simulate all agents inline — less depth, but faster for quick decisions.
- The `skills/csuite-research.md` prompt works great standalone in Step 1 before any board session if you want a proper research brief first.

## Design Principles

- **Context-neutral** — no personal context hardcoded; injected at runtime
- **Modular** — each agent usable standalone or orchestrated via the Moderator
- **Real subagent orchestration** — Round 1 runs in parallel via the Agent tool, no roleplay
- **Discussion over consensus** — Round 2 requires direct contradiction, topic shifts, or asymmetry
- **Claude Code first** — primarily a skill system; Opus 4.7 recommended for board sessions, Sonnet 4.6 for individual agents

More background in [CONTEXT.md](CONTEXT.md).
