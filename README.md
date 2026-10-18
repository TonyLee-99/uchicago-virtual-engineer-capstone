# Virtual Engineer — UChicago Capstone (Office of the CTO)

**MS in Applied Data Science capstone, University of Chicago · Feb 2026 – Dec 2026 · team of 4**

> This repository is a public summary. The code and data live in a private team repository and in the University's cloud project. They are not shared here because they contain facilities and building-automation data.

## The problem

UChicago's building automation systems (BAS) generate far more alarms than engineers can investigate. In the team's exploratory analysis of Johnson Controls Metasys data, the campus produced about 8,000 alarm triggers a day. Most of them (about 83%) came from field controllers going offline, not from equipment faults. The real equipment problems get buried in that noise, so maintenance stays mostly reactive.

**Virtual Engineer** is an LLM-assisted system that does the desk work an engineer does before opening a work order:

- filters alarms into cases worth investigating;
- pulls the relevant sensor history and maintenance records;
- explains likely causes, with evidence and uncertainty;
- drafts a prioritized work order for a human to approve.

The system is **read-only**: it never changes setpoints or controls equipment.

## Data

| Source | What it provides |
|---|---|
| Johnson Controls Metasys | historian readings, alarms/events, audit trails (campus-wide) |
| Siemens Desigo CC / Insight | readings, alarms, point catalogs, user views |
| IBM Maximo | maintenance work orders and cost transactions |

These sources add up to roughly 380 tables and about 15 billion rows, consolidated in **Google BigQuery**. The pilot covers four campus buildings and their air-handling units (AHUs).

## Architecture (current plan)

1. **Data stream.** Historical data is replayed as if live; real-time BAS APIs are not available during the capstone.
2. **Trigger layer.** Decides *when* an alarm or sensor pattern is worth an investigation. It groups alarm bursts, separates controller-offline noise from process alarms, and checks the persistence of the condition.
3. **Agent.** Investigates a case using BAS readings, alarm history, technical guidance, and Maximo records.
4. **Backend.**
5. **Frontend.** Engineer-facing review of cases and work-order drafts.

## My contributions

- **Trigger layer** (in review): analysis and code for deciding when to start an investigation from BAS alarm streams. It filters controller-offline alarm bursts and sets per-alarm-type persistence rules, so that only a small set of review cases reaches the agent.
- **Siemens Desigo sensor-granularity grouping** (merged): classified Desigo point names into building, equipment, and point-role levels to support sensor-to-equipment mapping.
- **Exploratory analysis** of alarm behavior and data quality across Metasys and Desigo.
- Tools: SQL (BigQuery), Python (pandas), Google Cloud.

## Status

The project is in active development. Component V1s are complete and are being integrated, ahead of the November 2026 Capstone Showcase and the December 2026 final paper. Results are evaluated with the facilities engineers; this page will be updated with outcomes once they are confirmed.
