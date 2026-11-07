# Project 1: AI Email and Calendar Assistant

An n8n workflow in which a Claude agent rewrites rough email drafts into polished messages and creates Google Calendar events from a chat request.

## How it works

`Chat message > AI Agent (Claude) > Gmail send / Google Calendar event`

- **When chat message received:** a chat window where you type a draft and a recipient.
- **AI Agent:** a system prompt tells the agent to rewrite the draft with a clear subject line and a concise, professional body, to send one email per person only, and to sign off with a fixed signature.
- **Simple Memory:** keeps the last 15 messages, so follow-ups such as "make it shorter" work.
- **Send a message in Gmail** and **Create an event in Google Calendar1:** the two tools the agent can call.

## Setup

1. In n8n, create a new workflow and choose Import from file: `email-calendar-assistant.json`.
2. Select your own Anthropic, Gmail and Google Calendar credentials.
3. In the Google Calendar node, choose your calendar.
4. In the AI Agent node, change the signature at the end of the system prompt if you want a different name.
5. Test with your own email address first. The agent sends real emails.

## Security notes

- The chat trigger is set to **not public** in this copy. Keep it that way unless you add access control; a public chat link would let anyone use your Gmail through the agent.
- Credentials, webhook IDs and personal addresses were removed before publishing.

## Limitations

A personal learning project, not a production system. There is no approval step before sending; for real use, add a review step.
