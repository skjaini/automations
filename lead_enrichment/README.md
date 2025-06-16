# 📧 n8n Sales Workflow Automation

This workflow classifies incoming emails, extracts lead data, enriches it with LinkedIn insights, stores leads in Airtable, and drafts intelligent replies using OpenAI and a Supabase knowledge base.

---

## 🧠 Features

- ✅ Email classification: Finances, Promotions, or High-Priority Leads  
- 📥 Gmail integration: Read, label, and process incoming messages  
- 🤖 OpenAI integration: Classify content, summarize leads, and generate replies  
- 🔗 LinkedIn enrichment: Uses Tavily to search for LinkedIn profiles  
- 📊 Airtable CRM: Adds or updates lead records with full context  
- 🧾 Telegram alerts: Sends team notifications for high-value leads or new meetings  
- 🧠 Supabase Vector Store: References company FAQ and service details to generate smart replies

---

## 🔄 Workflow Overview

### 1. Gmail Trigger
- Monitors inbox and retrieves new emails.
- Filters Calendly meeting notifications vs. other leads.

### 2. Email Classification
- Classifies content into:
  - **Finances**: invoices, statements, payments
  - **Promotion**: newsletters, offers, announcements
  - **New Leads**: urgent, actionable requests

### 3. Lead Data Extraction (LangChain)
- Extracts:
  - Name
  - Email address
  - Intent / how we can help

### 4. LinkedIn Enrichment
- Queries Tavily to find the lead's LinkedIn profile.
- Summarizes job role and company.

### 5. Airtable CRM Integration
- Adds new leads with:
  - Contact info
  - LinkedIn URL
  - Notes and summary
  - Lead status: `New Lead` or `Meeting Scheduled`
- Updates notes if lead already exists

### 6. Smart Response Generator
- Uses Supabase Vector Store (FAQs, services, testimonials) to:
  - Draft a contextual email
  - Include suggested subject + body
  - Save to Gmail as a draft

### 7. Notifications
- Telegram alerts for:
  - New leads
  - Calendly meeting signups
  - Drafted emails ready for approval

---

## 🛠 Setup Instructions

### 1. Gmail
- Add Gmail Trigger
- Set up label IDs:
  - `Label_2153712281606784851` (Promotion)
  - `Label_7937723162220600156` (Finances)
  - `Label_3986520172113512590` (Leads)

### 2. OpenAI
- Add API key in Credentials
- Use `gpt-4o-mini` model or upgrade to GPT-4 for more accurate classification

### 3. Airtable
- Set up your base and table
- Required fields:
  - Name
  - Email Address
  - LinkedIn URL
  - Notes
  - Lead Status
  - Lead Summary

### 4. Supabase Vector Store
- Upload FAQs, services, testimonials into a `documents` table
- Set up embedding with OpenAI

### 5. Tavily API
- Create account and retrieve API key
- Used to fetch LinkedIn summaries

### 6. Telegram
- Set `chatId` for your internal notification group

---

## 📌 Tags

`n8n` `OpenAI` `Supabase` `Airtable` `Tavily` `Gmail` `Telegram` `Sales Automation` `CRM` `Lead Qualification`

---

## ✅ Benefits

- Reduces manual CRM entry  
- Classifies and routes emails with precision  
- Automatically enriches lead data  
- Drafts personalized replies based on knowledge base  
- Keeps the team informed in real-time

---

## 🧪 Sample Email Snippet for Testing

```json
{
  "from": {
    "value": [
      {
        "address": "john@example.com",
        "name": "John CEO"
      }
    ]
  },
  "text": "Hi team, I’d love to schedule a time to talk about how you can help my ecommerce site increase conversions. Let me know when you're available."
}
