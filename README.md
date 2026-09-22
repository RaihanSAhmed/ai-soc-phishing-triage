# 🛡️ AI-Powered SOC Phishing Triage & SOAR Pipeline

> An automated Security Orchestration, Automation, and Response (SOAR) workflow built with n8n that enriches suspicious URLs, performs AI-assisted threat triage, and delivers structured incident reports to a SOC inbox.

## 📌 Project Overview

Tier-1 SOC analysts can experience alert fatigue from manually validating suspicious links, checking threat intelligence platforms, and documenting investigation results.

This project automates the ingestion, enrichment, triage, and response workflow using a self-hosted **n8n** instance.

When a suspicious URL is submitted, the pipeline:

1. Queries **VirusTotal** for URL reputation and detection data.
2. Submits the URL to **URLScan.io** for browser-based analysis and telemetry.
3. Normalizes the resulting security telemetry into a structured payload.
4. Uses a **Google Gemini-powered AI SOC Analyst** to analyze the evidence and generate a structured triage report.
5. Identifies **Key Risk Indicators (KRIs)** and recommended SOC actions.
6. Automatically delivers the resulting incident report as an HTML email via SMTP.

---

## 🏗️ Architecture & Data Flow

```text
[ Trigger / URL Input ]
           │
           ▼
┌─────────────────────────────────────────┐
│  Threat Intelligence Enrichment         │
│  • Base64 URL Encoding & VirusTotal API │
│  • URLScan.io Browser-Based Scan        │
└────────────────────┬────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────┐
│  Data Normalization                     │
│  • Extract & Standardize Telemetry      │
└────────────────────┬────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────┐
│  AI Threat Analysis (Google Gemini)     │
│  • Threat Verdict & Targeted Brand      │
│  • Key Risk Indicators (KRIs)           │
│  • SOC Remediation Action Matrix        │
└────────────────────┬────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────┐
│  Automated Alert Dispatch               │
│  • HTML Incident Report via SMTP        │
└─────────────────────────────────────────┘
