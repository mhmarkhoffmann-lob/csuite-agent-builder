# C-Suite Agent Builder

**Goal:** Build a modular, context-neutral agent system that serves as a personal "Board of Directors" for business, career, and personal decisions — built for Claude Code, individual agents independently distributable.

---

## Architecture

### Moderator (Orchestrator)
- Entry point for every board session
- Analyzes topic and scope
- Selects 2–4 fitting C-suite agents from the pool
- Facilitates structured discussion (2 rounds + synthesis)
- Interaction format: discussion — agents react to each other in Round 2

### C-Suite Agent Pool (11 Agents)

Each agent is an independent, distributable Claude Code skill:

- **CEO** — Overall strategy, vision, long-term value
- **CFO** — Finance, ROI, risk; sub-skills: financial analysis, ROI modeling, risk analysis
- **COO** — Operations, feasibility, resources
- **CHRO** — Career, leadership, culture, people
- **Coach** — Personal development, values, clarity
- **Chief Strategist** — Long-term strategy, market, trends (10-year horizon)
- **CAIO** — AI-first perspective, disruption, automation
- **CIO** — IT architecture, data, systems, scalability
- **Investor** — External contrarian view, investment thesis, market opportunity
- **Ethicist** — Ethical implications, human impact, values (practical, not abstract)
- **CSUO** — Sustainability, ESG, ecological and social impact

### Moderator Logic (Examples)

- Career decision → CHRO + Coach + Chief Strategist
- Evaluate a business project → CFO + Investor + CEO
- Tech investment → CIO + CAIO + CFO
- Personal priorities → Coach + Ethicist + Chief Strategist
- Sustainability question → CSUO + Ethicist + CFO
- Product launch → CEO + Investor + CMO

---

## Design Decisions

- **Context-neutral:** No personal context hardcoded — injected at runtime by the user. Enables sharing with others.
- **Modular:** Each C-suite agent is an independent skill — usable standalone or orchestrated by the Moderator.
- **Discussion format:** 2 rounds + synthesis. Round 1: independent assessment. Round 2: agents react to each other. Synthesis: Moderator summarizes, names tensions, gives recommendation.
- **Extensible:** Sub-skills per agent added incrementally (e.g., CFO → analysis skill, budget model skill).
- **Claude Code first:** Primarily a Claude Code skill system; optionally standalone later.
- **Recommended model:** Opus 4.7 for board sessions (quality > speed for real decisions). Sonnet 4.6 for development and lighter individual agents.
- **Deploy path:** Skills live in `skills/` in the project folder → deploy to `~/.claude/skills/csuite-[name]/`

---

## Session Flow

```
User: /csuite [topic]
        ↓
[Moderator] — Intake
        ↓
[Research Agent] — autonomous: determine dimensions → search → synthesize
        ↓
User confirms Session Context Package
        ↓
[Moderator] — Board selection + context injection
        ↓
Round 1: Independent assessment per agent
        ↓
Round 2: Discussion (agents react to each other)
        ↓
Synthesis: Consensus + tensions + recommendation
```

Research Agent also usable standalone: `/csuite-research [topic]`

## Skill File Structure

```
c-suite-agent-builder/
├── CONTEXT.md
├── README.md
├── skills/
│   ├── csuite-moderator.md    → /csuite
│   ├── csuite-research.md     → /csuite-research (also standalone)
│   ├── csuite-ceo.md
│   ├── csuite-cfo.md
│   ├── csuite-coo.md
│   ├── csuite-chro.md
│   ├── csuite-coach.md
│   ├── csuite-strategist.md
│   ├── csuite-caio.md
│   ├── csuite-cio.md
│   ├── csuite-investor.md
│   ├── csuite-ethiker.md
│   └── csuite-csuo.md
└── notes/
```

---

## GitHub Research (12.05.2026)

No direct equivalent found. Relevant references:

- **auto-company** (nicepkg/auto-company) — 14 agents with celebrity personas, fully autonomous 24/7, Claude Code. No personal advisor focus.
- **ivfarias/ceo** — 7 agents, software-dev focus, multi-platform.
- **DEV.to Solo Company** — 8 agents as `.agent.md`, knowledge graph. Closest to the vision, but SaaS-oriented.

**Differentiation:** No project combines adaptive board-selection moderator + personal & business advisory + modular distributable individual agents + discussion format.

---

## Open Items

- Sub-skills per agent (added incrementally)
- Slash command naming: `/csuite` fixed, `/csuite-research` fixed
- Live test → validate persona quality + discussion dynamics
- Deploy process: copy skills to `~/.claude/skills/csuite-[name]/`
