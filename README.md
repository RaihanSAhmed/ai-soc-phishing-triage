# 🛡️ AI-Powered SOC Phishing Triage & SOAR Pipeline

> **An automated Security Orchestration, Automation, and Response (SOAR) workflow built in n8n, leveraging VirusTotal, URLScan.io, and Google Gemini for real-time threat intelligence enrichment and AI incident triage.**
[ Trigger / URL Input ]
           │
           ▼
┌─────────────────────────────────────────┐
│  Threat Intelligence Enrichment         │
│  • Base64 Encoding & VirusTotal API v3  │
│  • URLScan.io Headless Scan & Retrieval  │
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
│  AI Threat Analysis (Google Gemini)    │
│  • Verdict & Targeted Brand             │
│  • Key Risk Indicators (KRIs)           │
│  • SOC Remediation Action Matrix        │
└────────────────────┬────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────┐
│  Automated Alert Dispatch               │
│  • Direct SMTP HTML Email Delivery      │
└─────────────────────────────────────────┘
