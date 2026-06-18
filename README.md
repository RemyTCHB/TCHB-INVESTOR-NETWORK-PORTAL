<div align="center">

<img src="icon.svg" alt="TCHB Investor Network Portal" width="120" height="120" />

# TCHB Investor Network Portal

### A secure, invite-only deal room for the Total Cash Home Buyers investor network

Exclusive, NDA-protected property data delivered to vetted investors — gated by per-property access keys and email one-time-passcode (OTP) verification, with full access auditing behind the scenes.

<br/>

![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-4285F4?style=for-the-badge&logo=google&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla%20JS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

</div>

---

## 📌 Overview

The **TCHB Investor Network Portal** is a confidential property data room for the Total Cash Home Buyers (TCHB) investor network. Investors browse a live catalog of available deals, then unlock the full underwriting package for any property by verifying their identity with a **per-property access key** plus an **emailed one-time passcode**.

Once unlocked, each property opens a deal-type-aware breakdown — **Wholesale / Off-Market**, **Subject-To / Creative**, or **Rental / Turnkey** — complete with financial tables and interactive charts. Every access attempt and viewing session is logged for a complete audit trail.

The entire system runs **serverless and zero-maintenance**: the frontend is a static Progressive Web App, and the backend is powered by **Google Apps Script + Google Sheets**, so the team manages deals and access from a familiar spreadsheet with no database to host.

---

## ✨ Features

### Secure two-step access
- **Per-property access keys** — every deal has its own confidential key; one key never unlocks another property.
- **Email OTP verification** — investors receive a branded 6-digit one-time passcode to confirm their identity before any data is shown.
- **"Request Access Key" flow** for prospects who don't yet have credentials, routed straight into the CRM.
- **Internal admin bypass** for authorized TCHB staff on the company domain.

### Live, spreadsheet-driven catalog
- Properties are pulled in real time from a **Google Sheet** via a JSON feed — add, edit, or pull a deal straight from the sheet with **no redeploy**.
- Dynamic column mapping means you can add or rename spreadsheet columns without touching code.

### Deal-aware data room
- Tailored layouts and metrics per strategy: **Wholesale**, **Subject-To / Creative**, and **Rental / Turnkey**.
- Financial breakdowns — purchase price, ARV, rehab budget, fees, spread, and projected returns.
- **Chart.js** visualizations for equity growth, cost breakdown, and side-by-side deal comparisons.

### Built-in audit & analytics
- Every event — OTP sent, access granted, failed attempt, admin bypass — is logged to a **Portal Access Logs** sheet.
- **Session tracking** records view start/end times and flags **bounces** (sessions under two minutes).
- **CRM webhooks** (LeadConnector / GoHighLevel) fire on key actions to capture investor intent.
- **Vercel Analytics + Speed Insights** for traffic and performance monitoring.

### Polished, app-like experience
- Installable **PWA** — standalone display, maskable icons, themed splash.
- Immersive dark glassmorphism UI, animated text gradients, custom cursor with a reactive mouse glow, and branded HTML email templates.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML5, Vanilla JavaScript (multi-page static app) |
| **Styling** | Tailwind CSS (CDN), Plus Jakarta Sans |
| **Charts** | Chart.js |
| **Backend** | Google Apps Script (serverless Web Apps) |
| **Database** | Google Sheets |
| **Email** | Gmail (via `GmailApp`) for OTP delivery |
| **CRM / Automation** | LeadConnector / GoHighLevel webhooks |
| **Analytics** | Vercel Analytics & Speed Insights |
| **App shell** | Progressive Web App (Web App Manifest) |
| **Hosting** | Vercel (static frontend) |

---

## 📁 Project Structure

```
.
├── index.html                          # Property catalog + authentication gate (email + key + OTP)
├── property_data.html                  # Confidential deal data room (renders after unlock)
├── manifest.json                       # PWA manifest (installable app metadata)
├── icon.png / icon.svg                 # App icons
├── logo.png / logo.svg                 # Brand assets (used in UI + emails)
├── package.json                        # Vercel analytics/speed-insights dependencies
├── pnpm-lock.yaml                      # pnpm lockfile
│
└── apps-script/                        # Google Apps Script backend (deployed as Web Apps)
    ├── Investor_Portal_Database_Script # doGet() — serves the "Database" sheet as JSON
    └── Investor_Portal_Logs_Script     # doPost() — OTP emails, access logging, session tracking
```

### How a deal gets unlocked

```
Investor opens portal
        │
        ▼
Browse live catalog  ──►  Pulled from Google Sheet (doGet → JSON)
        │
        ▼
Enter email + property access key
        │
        ▼
Receive 6-digit OTP by email  ──►  Apps Script (doPost → GmailApp)
        │
        ▼
Verify OTP  ──►  Logged to "Portal Access Logs" sheet
        │
        ▼
Redirect to property_data.html?id=…  ──►  Deal-aware data room + session tracking
```

---

## 🚀 Getting Started

### Prerequisites
- A **Google Account** with a Google Sheet for the property database
- A static host (e.g. **Vercel**) for the frontend
- Node.js + **pnpm** (only for the optional Vercel analytics packages)

### 1. Set up the Google Sheet backend
1. Create a Google Sheet with a **`Database`** tab. Add a header row including a `Property_ID` column and any deal fields you want (ARV, repair budget, access password, deal type, etc.).
2. Open **Extensions → Apps Script** and create two Web Apps:
   - **Database Script** — paste in `Investor_Portal_Database_Script` (`doGet`), which serves the sheet as JSON.
   - **Logs Script** — paste in `Investor_Portal_Logs_Script` (`doPost`), which sends OTP emails and writes to a `Portal Access Logs` tab.
3. **Deploy** each as a Web App (*Execute as: Me*, *Who has access: Anyone*) and copy the two `/exec` URLs.

### 2. Wire up the frontend
In `index.html` (and `property_data.html`), point the portal at your deployed endpoints — the Database feed, the Logs/OTP endpoint, and your CRM webhook.

### 3. Install dependencies & run
```bash
pnpm install
```

### 4. Deploy
Connect the repository to **Vercel** (or any static host). Vercel auto-deploys on every push to `main`.

---

## 🔒 Security Notes

This portal layers several protections — per-property keys, email OTP, domain-restricted admin access, full audit logging, and base64-obfuscated endpoint references to discourage casual inspection.

> ⚠️ **Important design consideration:** because the frontend is fully static, the property JSON feed (including access keys) and the OTP check happen **client-side**, and the admin bypass credential lives in the page source. This is fine for a low-friction, gentleman's-agreement investor portal protected by NDAs, but it is **not** a hard security boundary — a technical user with browser dev tools could inspect the returned data. If you ever need to guarantee that keys or unreleased deal data never reach an unauthorized browser, move the key check and OTP validation **server-side** (e.g. have Apps Script validate the key and only then return that property's data), and rotate the admin credential out of the client. Treat the current model as access *friction + auditing*, not encryption.

---

## 📄 License

Internal proprietary tool — © Total Cash Home Buyers. All rights reserved. All property data is confidential and subject to active non-disclosure agreements.

<div align="center">
<br/>
<sub>Built for the TCHB Investor Network 🏠💲</sub>
</div>
