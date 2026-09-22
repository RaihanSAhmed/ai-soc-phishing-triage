# 🛡️ AI-Powered SOC Triage Pipeline

An automated SOC workflow built with **n8n** that enriches suspicious URLs using **VirusTotal** and **URLScan.io**, uses **Google Gemini** for AI-assisted threat analysis, and automatically dispatches structured incident reports.

---

## 📌 Project Overview

Tier-1 SOC analysts can spend significant time manually validating suspicious links, checking threat intelligence platforms, analyzing web-page behavior, and documenting investigation results.

I built this automated pipeline using self-hosted **n8n** to streamline the initial phishing triage process.

When a suspicious URL is submitted, the workflow:

- Queries **VirusTotal** for URL reputation and detection data
- Submits the URL to **URLScan.io** for browser-based analysis
- Extracts and normalizes relevant security telemetry
- Passes the normalized evidence to a **Google Gemini AI SOC Analyst**
- Identifies **Key Risk Indicators (KRIs)** and potential targeted brands
- Generates recommended SOC response actions
- Automatically sends an HTML incident report through SMTP

---

## 🏗️ Architecture

```text
Suspicious URL
      │
      ▼
┌──────────────────────┐
│   n8n Manual Trigger │
└──────────┬───────────┘
           │
           ▼
┌─────────────────────────────┐
│   URL Encoding / Ingestion  │
│   • Base64 URL Encoding     │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│ Threat Intelligence         │
│                             │
│ • VirusTotal API v3         │
│ • URLScan.io API            │
│ • Reputation / DOM Data     │
│ • Redirects / Screenshots   │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   Telemetry Normalization   │
│   • Extract relevant fields │
│   • Standardize payload     │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   AI SOC Analyst            │
│   • Google Gemini           │
│   • Threat Verdict          │
│   • KRIs                    │
│   • Targeted Brand          │
│   • SOC Actions             │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   Incident Report           │
│   • HTML Formatting         │
│   • SMTP Delivery           │
└─────────────────────────────┘

## 🏗️ Architecture & Tech Stack

**Orchestration Engine:** n8n (Self-Hosted)
**Threat Intelligence:** VirusTotal API v3, URLScan.io API
**AI Engine:** Google Gemini
**Notification:** Gmail SMTP
**Techniques:** SOAR automation, REST API integration, Base64 URL encoding, DOM analysis, LLM prompt engineering

### Workflow

```text
Suspicious URL
      ↓
Base64 URL Encoding
      ↓
VirusTotal + URLScan.io
      ↓
Telemetry Normalization
      ↓
Google Gemini AI SOC Analyst
      ↓
Threat Verdict + KRIs + SOC Actions
      ↓
HTML Incident Report
      ↓
SMTP Delivery
```

## ⚙️ Implementation Phases

### Phase 1 — Target Ingestion & Encoding

The workflow is manually triggered by an analyst submitting a suspicious URL. A JavaScript Code Node converts the raw URL into an unpadded Base64 URL-safe string required by the VirusTotal API v3 URL endpoint.

---

### Phase 2 — Threat Intelligence Enrichment

The pipeline uses two threat intelligence sources to enrich the submitted URL.

**VirusTotal:**
Sends an authenticated `GET` request to retrieve available URL reputation, vendor detections, threat classifications, and related metadata.

**URLScan.io:**
Submits the URL for browser-based analysis, waits for the asynchronous scan to complete, and retrieves available telemetry such as DOM elements, redirects, infrastructure information, and screenshots.

---

### Phase 3 — Data Normalization & AI Threat Triage

A Set/Edit Fields node extracts relevant information from the raw VirusTotal and URLScan.io JSON responses and packages it into a normalized telemetry schema.

This normalized data is passed to a LangChain AI Agent powered by Google Gemini. The agent evaluates the available evidence against SOC triage guidelines and generates structured threat verdicts, Key Risk Indicators (KRIs), and recommended SOC actions.

**Example Normalized Telemetry:**

```json
{
  "PhishingLink": "https://example.com/login.html",
  "VT_Results": "16 vendor detections",
  "URLScan_Results": {
    "TargetBrand": "Microsoft",
    "DOM_Inputs": [
      "password-input"
    ],
    "Screenshot": "https://urlscan.io/screenshots/example.png"
  }
}
```

---

### Phase 4 — Automated Incident Dispatch

The AI-generated analysis is formatted into an HTML security alert. An SMTP node then authenticates over port `465` using SSL/TLS and Gmail App Passwords to deliver the finalized incident report to the configured SOC inbox.

```

I removed the **“70+ security engines”** claim here too since you don't need it for the README, and it can become outdated.
```

## 🔐 Security Relevance

This project demonstrates how repetitive phishing triage tasks can be automated and connected into a single SOC workflow.

Instead of manually switching between multiple security tools, an analyst can submit a suspicious URL and receive enriched threat intelligence, normalized telemetry, AI-assisted analysis, and a structured incident report through one workflow.

The pipeline demonstrates a practical SOAR architecture:

```text
Suspicious URL Input
        ↓
URL Encoding
        ↓
Threat Intelligence Enrichment
(VirusTotal + URLScan.io)
        ↓
Telemetry Normalization
        ↓
AI Threat Triage
(Google Gemini)
        ↓
KRI Identification
        ↓
Recommended SOC Actions
        ↓
Automated HTML Incident Report
        ↓
SOC Inbox
```

The AI component is designed to assist with initial triage rather than replace analyst judgment. Recommended response actions should be reviewed and validated by a human analyst before being applied to production systems.

## 🧪 Verified Execution Example

The workflow was tested against a suspicious URL and successfully completed the enrichment, analysis, and reporting pipeline.

### Target Evaluated

```text
https://notifyhubss.net/e236a5b10w19bb4b10e92543dc6q2d18eca6.html
```

### Threat Intelligence Results

**VirusTotal**

```text
16 security vendor detections
Classification: Phishing / Malicious
```

**URLScan.io**

```text
Potential targeted brand: Microsoft
DOM indicator: password-input
```

Additional analysis identified Microsoft-related visual assets and suspicious JavaScript behavior.

### AI SOC Analysis

The normalized telemetry was passed to the Google Gemini-powered SOC Agent, which generated a structured report containing:

* Threat classification
* Confidence level
* Potential targeted brand
* Key Risk Indicators (KRIs)
* Supporting evidence
* Recommended SOC actions

### Automated Incident Dispatch

The workflow generated an HTML incident report and automatically delivered it to the configured SOC inbox through SMTP.

Recommended response actions included:

* Domain blocking
* Mailbox investigation / purge
* Credential reset recommendations

> **Note:** AI-generated classifications and remediation recommendations are intended to assist analysts and should be validated before being applied to production systems.


