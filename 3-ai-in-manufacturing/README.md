# Project 3: AI in Manufacturing (AI Automation and AI Transformation)

Built from the manufacturing chapter of OECD (2026), *Progress in Implementing the European Union Coordinated Plan on Artificial Intelligence (Volume 2): Uptake in High-Impact Sectors*, https://doi.org/10.1787/3ac96d41-en (CC BY 4.0).

**Status: prototype.** The workflows are built for n8n with Claude. The deterministic logic (rule checks, defect rate, stock cover, scoring, safety guards) was tested with sample data. The workflows have not yet been imported and run end to end in n8n, and all limits are illustrative.

## 1. What the report says (the problem this project answers)

- Manufacturing employs over 30 million people in the EU and generates about 16% of value added.
- The share of manufacturing enterprises using at least one AI technology rose from 7% to 11% between 2021 and 2024. Adoption is fragmented; pharmaceuticals and electronics lead, while food processing, textiles and basic metals lag.
- Most AI use is language or administrative (text mining 4.6%, text generation 3.5%). Core-production uses are low: robotic process automation 3.2%, image recognition 2.7%, ML for data analysis 2.7%.
- Reasons for not adopting (2024): lack of expertise (over 7.5%), data availability or quality (5.0%), legal consequences (4.9%), incompatibility with existing systems (4.8%), data protection (4.4%), cost (3.9%). Less than 2% of the manufacturing workforce has AI skills in most Member States.
- The report highlights three high-impact use-cases: **predictive maintenance, quality assurance and control, supply chain optimisation**.
- Its recommendations: interoperable data sharing, practical guidance on the EU AI Act and GDPR, upskilling co-created with industry, and building trust through success stories and explainability.

## 2. Project design

| Report finding | Workflow | What it does |
|---|---|---|
| Predictive maintenance is the most mature use-case | **1 Predictive Maintenance Monitor** | Webhook receives a sensor reading. Rules flag anomalies. Claude ranks the factors and recommends an action. High and Critical cases email the maintenance manager. |
| Quality assurance and control | **2 Quality Assurance Assistant** | Webhook receives an inspection result. Code calculates the defect rate. Claude suggests a disposition (Release, Hold, Reject), causes and actions. |
| Supply chain optimisation | **3 Supply Chain Weekly Brief** | Every Monday, reads the inventory sheet, calculates stock cover versus lead time, and emails a plain-language briefing. |
| Skills, data, regulation and trust barriers | **4 AI Readiness Assessment** | A form scores a company on data, infrastructure, skills, regulation and leadership. Claude drafts a 90-day roadmap grounded in the report. The draft goes to you for review first. |

Design principles taken from the report:
1. **Explainability.** Workflows 1 and 2 ask the AI to rank the factors behind its recommendation, so operators can see why.
2. **Deterministic guard.** The report describes a robot that misread an object, and a monitoring system that validates AI output before action. The "Validate AI Output" Code node does this: the AI can never lower a hard-limit breach below High (workflow 1) or release a batch above the hard defect limit (workflow 2), and invalid output is escalated to a person.
3. **Human in the loop.** The AI recommends; engineers decide. Workflow 4 sends a draft to you before anything reaches a client.
4. **Arithmetic in code, language in AI.** Calculations are done in Code nodes; the AI only interprets and writes.

## 3. Files

- `1-predictive-maintenance.json`, `2-quality-assurance.json`, `3-supply-chain-brief.json`, `4-ai-readiness-assessment.json`: import each via n8n > Workflows > Import from file.
- `AI_Manufacturing_Project_Sheet_Template.xlsx`: import into Google Sheets. Tabs: Inventory, Maintenance Log, Quality Log, Readiness Log.

## 4. Setup (about 30 minutes)

1. Import the template into Google Sheets.
2. Import the four workflows into n8n.
3. In every workflow, select your Anthropic, Google Sheets and Gmail credentials, then choose the right spreadsheet and tab in each Google Sheets node.
4. Replace the placeholders: `REPLACE_WITH_YOUR_SECRET_KEY` (workflows 1 and 2), and the email placeholders in the Gmail nodes.
5. Adjust the illustrative limits in the "Rule Check" and "Calculate Defect Rate" nodes.
6. Test, then activate.

## 5. Test data

Use fictional data only. Replace the host and key.

Workflow 1 (should flag a hard-limit breach and alert):
```bash
curl -X POST "https://YOUR-N8N/webhook-test/pm-sensor-reading" \
  -H "Content-Type: application/json" -H "x-api-key: YOUR_SECRET_KEY" \
  -d '{"machine_id":"M-07","temperature_c":92,"vibration_mm_s":5.2,"pressure_bar":6,"runtime_hours":1200,"last_service_days":120}'
```

Workflow 2 (7% defects: the AI cannot release it):
```bash
curl -X POST "https://YOUR-N8N/webhook-test/qa-inspection-result" \
  -H "Content-Type: application/json" -H "x-api-key: YOUR_SECRET_KEY" \
  -d '{"batch_id":"B-1042","product_line":"Housing A","units_inspected":200,"units_defective":14,"defect_types":"scratch:9, misalignment:5","inspector_notes":"Scratches mostly on the left edge"}'
```

Workflow 3: fill the Inventory tab and click "Execute workflow". Workflow 4: open the form URL from the trigger node.

For production, use the `/webhook/` URL instead of `/webhook-test/` once the workflow is active.

## 6. Limitations (be honest about these)

- Prototype, not production. Not yet run end to end in n8n.
- Limits (temperature, vibration, pressure, runtime, and the 2% and 5% defect rates) are illustrative, not industry standards.
- Rules-based flags are not machine-learning predictions. Real predictive maintenance needs historical sensor data and a trained model; this project is the orchestration and decision-support layer.
- The readiness score is self-assessed and simple. The AI roadmap is a draft for a person to review.
- Do not send real customer or production data to a test environment. Keep real data within GDPR and your company policy.
- The report is a policy study of the EU, not a technical guide. Its statistics describe EU enterprises, not any single client.

## 7. Suggested 12-week plan to take it further

| Weeks | Focus | Output |
|---|---|---|
| 1-2 | Set up, import, test all four workflows | Working prototype, screenshots |
| 3-4 | Pick one use-case; interview a plant or business contact; collect real (anonymised) requirements | Requirements and KPI list |
| 5-8 | Replace illustrative limits with real ones; add error handling and logging; add a dashboard (Google Sheets or Looker Studio) | Pilot version |
| 9-10 | Review data protection and EU AI Act implications; document human oversight | Risk and compliance note |
| 11-12 | Pilot, measure, present | Case study with real results |

Never present results (savings, downtime reduction) until you have measured them.
