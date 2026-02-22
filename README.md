# Invoice-Email-Monitoring-and-Data-Extraction-for-Finance
# 🤖 Autonomous GenAI Invoice Processor

A sophisticated end-to-end automation pipeline that transforms cluttered Gmail inboxes into structured financial data. This project leverages **Generative AI** to automate the extraction, validation, and logging of invoices into Google Sheets with a **Slack-based human-in-the-loop** approval system.



## 🌟 Overview
Manual invoice entry is error-prone and slow. This engine automates the entire lifecycle:
1. **Fetch:** Targets invoice-related emails via the **Gmail API** (OAuth2).
2. **Extract:** Uses **OpenAI GPT-4o-mini** to parse structured JSON from raw email text and PDF attachments.
3. **Approve:** Routes high-priority data to **Slack** for real-time status updates and conditional approvals.
4. **Log:** Synchronizes all validated records into a **Google Sheets** central ledger.

## 🛠️ Technical Architecture

### 1. Intelligent Parsing (GenAI)
The core logic resides in a custom Node.js script that handles:
- **PDF Extraction:** Utilizes `pdf-parse` to read digital attachments.
- **Fallback Logic:** If a PDF is missing or unreadable, the engine automatically analyzes the email `subject` and `body` to ensure no invoice is missed.
- **Rate Limit Handling:** Implemented error catching for OpenAI `429` (Quota) errors to ensure the pipeline remains resilient under heavy loads.

### 2. Secure Integration
- **OAuth2 Handshake:** Configured custom Google Cloud Platform (GCP) credentials to manage scopes for both `gmail.readonly` and `spreadsheets`.
- **Environment Variables:** Secure management of Client IDs, Secrets, and Refresh Tokens to protect sensitive API access.

## 🚀 How It Works
1. **Trigger:** The bot polls the Gmail inbox for keywords like "Invoice" or "Receipt."
2. **AI Analysis:** The extracted text is sent to a fine-tuned GenAI prompt:
   ```json
   {
     "invoiceNumber": "INV-123",
     "amount": "500.00",
     "supplier": "Acme Corp",
     "date": "2026-02-22"
   }
