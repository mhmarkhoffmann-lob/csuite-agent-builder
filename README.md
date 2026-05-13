# C-Suite Agent Builder

Ein modulares "Board of Directors" als Claude Code Skill-System. Statt einem generischen Assistenten bekommst du ein adaptiv besetztes Board aus C-Suite-Perspektiven, das deine Entscheidung in zwei Diskussionsrunden plus Synthese durchleuchtet.

## Was es macht

`/csuite [thema]` startet eine Board-Session:

1. **Intake** — Moderator klärt Entscheidung, Rolle, Kontext
2. **Research** — autonomer Research-Agent bestimmt Wissensdimensionen, sammelt Fakten, liefert ein Session Context Package
3. **Board-Auswahl** — Moderator wählt 2–4 passende Agents aus dem Pool
4. **Runde 1** — jeder Agent gibt unabhängige Einschätzung (paralleler Subagent-Aufruf)
5. **Runde 2** — Agents reagieren aufeinander: Frontalwiderspruch, Themenwechsel, Asymmetrien
6. **Synthese** — Konsens, Spannungen, Empfehlung

Einzelne Agents sind auch standalone nutzbar (`/csuite-cfo`, `/research`, etc.).

## Agent-Pool

- **CEO** — Gesamtstrategie, Vision, langfristiger Wert
- **CFO** — Finanzen, ROI, Risiko
- **COO** — Operations, Umsetzbarkeit, Ressourcen
- **CHRO** — Karriere, Führung, Kultur
- **CIO** — IT-Architektur, Daten, Skalierbarkeit
- **CAIO** — AI-First-Perspektive, Disruption, Automatisierung
- **CSUO** — Nachhaltigkeit, ESG, ökologischer & sozialer Impact
- **Chief Strategist** — Langfriststrategie, Megatrends (10-Jahres-Horizont)
- **Investor** — externe Contrarian-View, Kill-Szenarien
- **Ethiker** — ethische Implikationen, Stakeholder, unbeabsichtigte Folgen
- **Coach** — persönliche Werte, Klarheit, Authentizität

Plus zwei Orchestrierer:

- **Moderator** (`csuite-moderator.md`) — adaptiver Board-Selector, Diskussionsleiter
- **Research** (`csuite-research.md`) — autonomer Vorab-Recherche-Agent

## Beispiel-Konstellationen

- Karriereentscheidung → CHRO + Coach + Chief Strategist
- Business-Projekt bewerten → CFO + Investor + CEO
- Tech-Investition → CIO + CAIO + CFO
- Persönliche Prioritäten → Coach + Ethiker + Chief Strategist
- Nachhaltigkeitsfrage → CSUO + Ethiker + CFO

## Installation

Skills nach `~/.claude/skills/csuite-[name]/` kopieren (oder als Plugin in einem Marketplace verpacken). Alle Skills sind reine Markdown-Dateien mit Frontmatter — kein Build-Schritt nötig.

```bash
git clone https://github.com/mhmarkhoffmann-lob/csuite-agent-builder.git
cd csuite-agent-builder
for f in skills/csuite-*.md; do
  name=$(basename "$f" .md)
  mkdir -p ~/.claude/skills/"$name"
  cp "$f" ~/.claude/skills/"$name"/SKILL.md
done
```

## Design-Prinzipien

- **Kontext-neutral** — kein persönlicher Kontext hardcoded, wird zur Laufzeit injiziert
- **Modular** — jeder Agent eigenständig nutzbar oder über Moderator orchestriert
- **Echte Subagent-Orchestrierung** — Runde 1 läuft parallel über das Agent-Tool, kein Roleplay
- **Diskussion statt Konsens** — Runde 2 verlangt Frontalwiderspruch, Themenwechsel oder Asymmetrie
- **Claude Code first** — primär als Skill-System; Opus 4.7 empfohlen für Board-Sessions, Sonnet 4.6 für Einzelagents

Mehr Hintergrund in [KONTEXT.md](KONTEXT.md).
