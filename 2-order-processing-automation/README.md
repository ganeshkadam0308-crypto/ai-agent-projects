# Project 2: Order Processing Automation

An n8n prototype that takes orders through a web form, assigns a priority, lets a Claude agent review the order, logs it in Google Sheets, and sends either a customer confirmation or an internal review request.

## Flow

`Form > Prepare Order (timestamp + priority) > AI Order Review (Claude) > Log order in sheet > Approved? > email`

- **Priority:** Normal = Medium, Urgent = High, Bulk = Low.
- **AI Order Review:** returns a structured result: status (Approved or Needs Review), a one-line summary, and a draft email.
- **Approved?** Approved orders email the customer. Others email an internal reviewer, so a person decides.

## Setup

1. Import `order-processing-automation.json` into a new n8n workflow.
2. Select your Anthropic, Google Sheets and Gmail credentials.
3. In **Log order in sheet**, choose a spreadsheet whose first row contains these headers: Submitted At, Customer Name, Customer Email, Order Type, Order Amount, Order Description, Priority, Status, AI Summary.
4. In **Email reviewer (needs review)**, replace `REPLACE_WITH_REVIEWER_EMAIL`.
5. The AI node's system prompt sends orders above 50,000 to review. This is an example rule; change it to your own.
6. Open the form URL from the trigger node and test with fictional orders.

## Limitations

A prototype. It has not been tested end to end with real orders, and the review rule is only an example.
