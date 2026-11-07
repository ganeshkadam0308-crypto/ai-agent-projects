# AI Agent Projects

Three AI-agent projects built with [n8n](https://n8n.io) and Claude (Anthropic), by Ganesh Kadam. They explore how AI can take over routine coordination work while people keep control of decisions.

## Projects

| # | Project | What it shows |
|---|---|---|
| 1 | [AI Email and Calendar Assistant](1-email-calendar-assistant) | A tool-using agent that rewrites drafts into polished emails and schedules events from chat. |
| 2 | [Order Processing Automation](2-order-processing-automation) | Form intake, priority assignment, AI review, a Google Sheets log, and escalation of exceptions to a person. |
| 3 | [AI in Manufacturing](3-ai-in-manufacturing) | Four connected workflows (predictive maintenance, quality assurance, supply chain brief, AI readiness assessment) built from the manufacturing chapter of an OECD report. |

![AI in Manufacturing](images/project3-ai-in-manufacturing.png)

## Status

These are **prototypes and learning projects, not production systems.** The decision logic of project 3 was tested with fictional sample data. The workflows have not yet been run end to end with real data, all thresholds are illustrative, and no results or savings are claimed.

## How to use any workflow

1. Install or sign up for n8n.
2. Create a new workflow, open the menu, and choose **Import from file**.
3. Select your own credentials (Anthropic, Google Sheets, Gmail, Google Calendar). No credentials are included in this repository.
4. Replace every `REPLACE_WITH_...` placeholder.
5. Test with fictional data before connecting anything real.

## Design principles

- The AI recommends; a person decides.
- A code check runs after the AI, so it cannot override hard safety limits.
- Calculations are done in code; the AI interprets and writes.
- Keep personal data and real customer data out of tests.

## Source

Project 3 is based on OECD (2026), *Progress in Implementing the European Union Coordinated Plan on Artificial Intelligence (Volume 2): Uptake in High-Impact Sectors*, https://doi.org/10.1787/3ac96d41-en (CC BY 4.0). The report itself is not included here.

## Author

Ganesh Kadam. LinkedIn: add your profile link here.

## License

MIT, see [LICENSE](LICENSE).
