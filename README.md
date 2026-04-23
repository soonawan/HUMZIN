# AI Humanizer — Deployment Guide

Complete step-by-step: empty repo → live on Vercel.

## What this app does
- Paste text or upload a .docx
- Rewrites only paragraphs and bullets using Groq (Llama 3 70B — free tier)
- Returns humanized text or a downloadable .docx
- Images, tables, headings, formatting: never touched

## Stack
| Layer | Tool | Cost |
|-------|------|------|
| Frontend + API | Next.js on Vercel | Free |
| LLM | Groq Llama 3 70B | Free |
| .docx parsing | JSZip + xml2js | — |

---

## STEP 1 — Get your free Groq API key
1. Go to https://console.groq.com
2. Sign up (free, no credit card)
3. Click API Keys → Create API Key
4. Copy the key

---

## STEP 2 — Push this code to GitHub
```bash
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

---

## STEP 3 — Connect Vercel to your repo
1. Go to https://vercel.com/dashboard
2. Add New → Project → Import your GitHub repo
3. Framework: Next.js (auto-detected)
4. Do NOT deploy yet

---

## STEP 4 — Add env variable in Vercel
On the Vercel import screen:
1. Expand "Environment Variables"
2. Name: GROQ_API_KEY | Value: (your key from Step 1)
3. Click Add
4. Click Deploy

---

## STEP 5 — Test
Your app is live at https://your-app.vercel.app

Text mode: paste text → Humanize text → result appears in ~3s
Docx mode: drop .docx → Humanize .docx → download in ~20s

---

## Local dev
```bash
npm install
cp .env.local.example .env.local
# paste your GROQ_API_KEY into .env.local
npm run dev
# open http://localhost:3000
```

---

## Troubleshooting
| Problem | Fix |
|---------|-----|
| "GROQ_API_KEY is not configured" | Add env var in Vercel → Settings → Environment Variables → redeploy |
| "No rewritable text found" | Doc uses text boxes; switch to text mode |
| Timeout on large .docx | Free Vercel has 10s limit; use Pro (60s) for long docs |
| Build fails | Run npm install locally, commit package-lock.json, push |

---

## File structure
```
pages/index.js          UI
pages/api/humanize.js   API route
lib/humanizer.js        Groq + prompt
lib/docxParser.js       .docx parse/rebuild
.env.local.example      copy for local dev
```

## Customize the prompt
Edit SYSTEM_PROMPT in lib/humanizer.js
