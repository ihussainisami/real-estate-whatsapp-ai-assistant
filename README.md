# Real Estate WhatsApp AI Assistant

An AI-powered WhatsApp assistant for UK real estate agencies. It handles property enquiries, qualifies leads, books viewings against a live calendar, and escalates only genuinely high-intent leads to a human agent.

Built with **n8n**, **Google Gemini**, **WhatsApp Business Cloud API**, **Airtable**, and **Google Calendar**.

## What it does

- Answers property questions (price, bedrooms, EPC rating, pet policy, and so on) using live data from Airtable. It never invents details.
- Qualifies leads as HOT, WARM, or COLD based on their stated timeline and intent.
- Checks real calendar availability before confirming a viewing, and re-verifies before every booking rather than trusting its own earlier replies.
- Transcribes and understands voice notes, not just text.
- Escalates HOT leads to a human agent via WhatsApp with a lead summary, deduplicated so one chatty lead doesn't spam the agent's phone.
- Falls back gracefully if the AI agent fails. The customer still gets a response, and the developer gets a separate alert.

## Architecture

Inbound WhatsApp message, then message-type routing (text or audio), then input normalization, then the AI agent (Gemini, with Airtable and Google Calendar as tools), then structured output (reply, lead status, escalation flag, appointment status). From there, the reply is sent, the lead is logged to Sheets, and if it's HOT, the agent is notified.

Error paths are handled explicitly. Agent failures trigger a customer-facing fallback message plus a separate developer alert, rather than failing silently.

## Stack

- **n8n**, workflow orchestration
- **Google Gemini**, conversation, lead qualification, audio transcription
- **WhatsApp Business Cloud API**, messaging channel
- **Airtable**, property database and single source of truth
- **Google Calendar API**, viewing availability and booking
- **Google Sheets**, lead log

## Notes

This is a portfolio project built with a synthetic property dataset and a test WhatsApp number, not connected to a live agency. Credentials and account IDs have been stripped from the exported workflow JSON.
