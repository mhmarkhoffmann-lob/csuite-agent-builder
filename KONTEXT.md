# C-Suite Agent Builder

**Ziel:** Ein modulares, kontext-neutrales Agentensystem aufbauen, das als persönliches "Board of Directors" für Business-, Karriere- und Privatentscheidungen dient — gebaut für Claude Code, einzelne Agents unabhängig verteilbar.

---

## Architektur

### Moderator (Orchestrator)
- Einstiegspunkt für jede Board-Session
- Analysiert Topic & Scope
- Wählt 2–4 passende C-Suite Agents aus dem Pool
- Moderiert strukturierte Diskussion (2 Runden + Synthese)
- Interaktionsform: Diskussion — Agents reagieren aufeinander in Runde 2

### C-Suite Agent Pool (11 Agents)

Jeder Agent ist ein eigenständiger, verteilbarer Claude Code Skill:

- **CEO** — Gesamtstrategie, Vision, langfristiger Wert
- **CFO** — Finanzen, ROI, Risiko; Sub-Skills: Finanzanalyse, ROI-Modellierung, Risikoanalyse
- **COO** — Operations, Umsetzbarkeit, Ressourcen
- **CHRO** — Karriere, Führung, Kultur, Menschen
- **Coach** — Persönliche Entwicklung, Werte, Klarheit
- **Chief Strategist** — Langfriststrategie, Markt, Trends (10-Jahres-Horizont)
- **CAIO** — AI-First-Perspektive, Disruption, Automatisierung
- **CIO** — IT-Architektur, Daten, Systeme, Skalierbarkeit
- **Investor** — Externe Contrarian-View, Investment-Thesis, Marktchance
- **Ethiker** — Ethische Implikationen, menschlicher Impact, Werte (nicht "Menschheit", sondern praxisnah)
- **CSUO** — Nachhaltigkeit, ESG, ökologischer & sozialer Impact

### Moderator-Logik (Beispiele)

- Karriereentscheidung → CHRO + Coach + Chief Strategist
- Business-Projekt bewerten → CFO + Investor + CEO
- Tech-Investition → CIO + CAIO + CFO
- Persönliche Prioritäten → Coach + Ethiker + Chief Strategist
- Nachhaltigkeitsfrage → CSUO + Ethiker + CFO
- Produkt-Launch → CEO + Investor + CMO

---

## Design-Entscheidungen

- **Kontext-neutral:** Kein persönlicher Kontext hardcoded — wird zur Laufzeit durch den Nutzer injiziert. Ermöglicht Weitergabe an andere.
- **Modular:** Jeder C-Suite Agent ist ein eigenständiger Skill — standalone nutzbar oder durch Moderator orchestriert.
- **Diskussionsformat:** 2 Runden + Synthese. Runde 1: unabhängige Einschätzung. Runde 2: Reaktion auf andere Agents. Synthese: Moderator fasst zusammen, benennt Spannungen, gibt Empfehlung.
- **Erweiterbar:** Sub-Skills je Agent werden schrittweise ergänzt (z.B. CFO → Analyzing-Skill, Budgetmodell-Skill).
- **Claude Code first:** Primär als Claude Code Skill-System; ggf. später auch standalone.
- **Empfohlenes Modell:** Opus 4.7 für Board-Sessions (Qualität > Geschwindigkeit bei echten Entscheidungen). Sonnet 4.6 für Entwicklung und leichte Einzelagents.
- **Deploy-Pfad:** Skills liegen in `skills/` im Projektordner → deployen nach `~/.claude/skills/csuite-[name]/`

---

## Session-Flow

```
User: /csuite [thema]
        ↓
[Moderator] — Intake
        ↓
[Research Agent] — autonom: Dimensionen bestimmen → suchen → synthetisieren
        ↓
User bestätigt Session Context Package
        ↓
[Moderator] — Board-Auswahl + Kontext-Injektion
        ↓
Runde 1: Unabhängige Einschätzung je Agent
        ↓
Runde 2: Diskussion (Agents reagieren aufeinander)
        ↓
Synthese: Konsens + Spannungen + Empfehlung
```

Research Agent auch standalone nutzbar: `/research [thema]`

## Skill-Dateistruktur

```
c-suite-agent-builder/
├── KONTEXT.md
├── FOCUS.md
├── skills/
│   ├── csuite-moderator.md    → /csuite
│   ├── csuite-research.md     → /research (auch standalone)
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

## GitHub-Recherche (12.05.2026)

Kein direktes Äquivalent gefunden. Relevante Referenzen:

- **auto-company** (nicepkg/auto-company) — 14 Agents mit Berühmtheits-Personas, vollständig autonom 24/7, Claude Code. Kein persönlicher Berater-Fokus.
- **ivfarias/ceo** — 7 Agents, Software-Dev-Fokus, Multi-Plattform.
- **DEV.to Solo Company** — 8 Agents als `.agent.md`, Knowledge Graph. Nächste an der Vision, aber SaaS-orientiert.

**Differenzierung:** Kein Projekt kombiniert adaptiven Board-Selection-Moderator + persönliche & Business-Beratung + modulare verteilbare Einzelagents + Diskussionsformat.

---

## Offene Punkte

- Sub-Skills je Agent (werden schrittweise ergänzt)
- Naming der Slash-Commands: `/csuite` fix, `/research` fix
- Live-Test durchführen → Qualität der Personas + Diskussionsdynamik prüfen
- Deploy-Prozess: Skills nach `~/.claude/skills/csuite-[name]/` kopieren
