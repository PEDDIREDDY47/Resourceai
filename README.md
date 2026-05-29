ResourceAI — Intelligent Workforce Allocation Platform
> **Band-aware · Availability-tiered · AI-matched · Production ready**
A fully client-side workforce resource management system. No backend required — runs entirely in the browser using `localStorage` with XOR encryption. Upload any Excel format and the AI maps columns automatically.
---
📁 Project Structure
```
ResourceAI/
├── index.html              ← Single entry point; links all modules in order
├── css/
│   └── styles.css          ← Full design system (tokens, components, layout)
└── js/
    ├── security.js         ← Password hashing (PBKDF2-inspired) + XOR encryption
    ├── database.js         ← Encrypted localStorage state manager
    ├── colmapper.js        ← AI column-header normalizer (any Excel format)
    ├── excel.js            ← SheetJS XLSX + native CSV parser
    ├── ai.js               ← Band engine (A1–D3) + tiered matching algorithm
    ├── config.js           ← Notification engine (SMTP stub / email log)
    ├── app.js              ← Routing, auth, all pages except requirements
    └── requirements.js     ← Requirements page (overrides + fixes)
```
Script load order is critical — each module depends on those above it.
---
✨ Features
Feature	Detail
🎯 Band Hierarchy	A1 → A2 → A3 → B1 → B2 → B3 → C1 → C2 → C3 → D1 → D2 → D3
🟢 Availability Tiers	Free → Partial → Ending 30-60d → Internal → Busy
🧠 AI Skill Matching	Concept synonyms, proficiency levels (Expert/Advanced/Beginner)
📊 Score Breakdown	Skill%, Band%, Exp%, Location%, Availability% shown per match
📂 Any Excel Format	Auto-maps 40+ column header variants per schema
🔐 Secure Auth	Password hashing, session encryption, approval workflow
📬 Email Log	All notifications logged; swap in SMTP for real delivery
📈 Reports	Utilization charts, skill heatmaps, 60-day forecast
---
🚀 Quick Start
Option A — Open directly
```bash
# Just open index.html in any modern browser
open index.html
```
Option B — Local dev server (recommended)
```bash
# Python
python3 -m http.server 8080

# Node
npx serve .

# Then visit http://localhost:8080
```
Default Admin Credentials
```
Email:    admin@resourceai.com
Password: Admin@2025
```
> Change these immediately after first login via Admin Center → Users.
---
📤 Uploading to GitHub
```bash
# 1. Create repo on github.com, then:
git init
git add .
git commit -m "feat: initial ResourceAI release"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ResourceAI.git
git push -u origin main
```
Deploy as GitHub Pages (free hosting)
Go to your repo → Settings → Pages
Source: Deploy from a branch → `main` → `/ (root)`
Your app is live at `https://YOUR_USERNAME.github.io/ResourceAI/`
---
📧 Free SMTP Options (to enable real email delivery)
Currently all emails are logged to Admin Center → Email Log. To send real emails, replace the `Notify.send()` stub in `js/config.js` with one of:
Option 1 — EmailJS (easiest, no backend)
```
Free tier: 200 emails/month
Setup: https://www.emailjs.com
```
```js
// In config.js, replace Notify.send():
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', {
  to_email: to, subject, message: body
});
```
Option 2 — Brevo (formerly Sendinblue)
```
Free tier: 300 emails/day
API docs: https://developers.brevo.com
```
```js
fetch('https://api.brevo.com/v3/smtp/email', {
  method: 'POST',
  headers: { 'api-key': 'YOUR_BREVO_KEY', 'Content-Type': 'application/json' },
  body: JSON.stringify({ sender:{email:'no-reply@yourdomain.com'},
    to:[{email:to}], subject, textContent:body })
});
```
Option 3 — Resend
```
Free tier: 3,000 emails/month, 100/day
API docs: https://resend.com/docs
```
```js
fetch('https://api.resend.com/emails', {
  method: 'POST',
  headers: { Authorization: 'Bearer YOUR_RESEND_KEY', 'Content-Type': 'application/json' },
  body: JSON.stringify({ from:'no-reply@yourdomain.com', to:[to], subject, text:body })
});
```
Option 4 — Cloudflare Workers + Gmail (zero cost at scale)
```
Free tier: 100,000 requests/day
Uses Gmail SMTP relay via Cloudflare Worker
Guide: https://developers.cloudflare.com/workers/tutorials/send-emails-with-resend/
```
Option 5 — SMTP.js (client-side, for dev/testing only)
```
NOT for production — exposes credentials in browser
```
```js
Email.send({ Host:'smtp.gmail.com', Username:'you@gmail.com',
  Password:'app-password', To:to, Subject:subject, Body:body });
```
> **Recommendation for production:** Use **Resend** (best DX) or **Brevo** (highest free limit).
> All require moving the API key to a backend (Cloudflare Worker / Vercel Edge Function)
> to avoid exposing it in client-side code.
---
📋 Excel Import Format
Skills Matrix
Name	Department	Designation	Experience	Location	Skills	Secondary Skills
Jane Doe	Engineering	B2	5	Hyderabad	React, Node.js	Docker, SQL
Allocation Data
Employee Name	Project	Allocation %	Start Date	End Date
Jane Doe	Project Alpha	50	2025-01-01	2025-06-30
Requirements
Role	Skills	Band	Min Experience	Location	Headcount	Priority
Senior React Dev	React, Node.js	B2	4	Hyderabad	2	High
> Headers are fuzzy-matched — "Required Skills", "Tech Stack", "Key Skills" all work.
---
🏅 Band Reference
Band	Level	Example Titles
A1	Fresher	Intern, Trainee, Graduate
A2	Junior Associate	Associate Developer
A3	Associate	Software Engineer I
B1	Mid	Software Engineer, Developer
B2	Senior Associate	Senior Software Engineer
B3	Senior	Senior Developer, SSE
C1	Lead	Tech Lead, Team Lead
C2	Senior Lead	Staff Engineer, Senior Lead
C3	Principal	Principal Engineer
D1	Architect	Solution Architect
D2	Manager	Engineering Manager
D3	Director	Director, VP, Head of
---
🔒 Security Notes
All data encrypted in `localStorage` via XOR + Base64
Passwords hashed with a dual-hash algorithm (never stored plain)
Sessions expire after 8 hours
Department isolation: non-admin users see only their department's data
No data ever leaves the browser
---
📄 License
MIT — free to use, modify, and distribute.
