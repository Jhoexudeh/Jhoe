# BrandMechanic AI — Vercel Deployment Guide

## What changed
The original HTML called Anthropic's API directly from the browser (blocked by CORS on Vercel).
This version routes all API calls through a secure Next.js serverless function at `/api/chat`.
Your API key is stored as a Vercel environment variable — never exposed to the browser.

---

## How to deploy

### Step 1 — Push to GitHub
1. Create a new repo on github.com (e.g. `brandmechanic-ai`)
2. Upload all files in this folder to the repo

### Step 2 — Import to Vercel
1. Go to vercel.com → New Project
2. Import your GitHub repo
3. Framework preset: **Next.js** (auto-detected)
4. Click **Deploy** (it will fail — that's expected, you need the env variable first)

### Step 3 — Add your API key
1. In Vercel → your project → **Settings** → **Environment Variables**
2. Add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** your key (starts with `sk-ant-...`)
   - **Environment:** Production, Preview, Development (check all three)
3. Click **Save**

### Step 4 — Redeploy
1. Go to **Deployments** tab
2. Click the three dots on the latest deployment → **Redeploy**
3. Your app is now live ✅

---

## File structure
```
brandmechanic-ai/
├── pages/
│   ├── index.js          ← redirects to the HTML app
│   └── api/
│       └── chat.js       ← serverless function (proxies Anthropic API)
├── public/
│   └── index.html        ← your full BrandMechanic AI app
├── package.json
├── next.config.js
└── README.md
```

---

## How it works
- Browser → calls `/api/chat` (your Vercel server)
- `/api/chat` → calls Anthropic with your secret API key
- Response → back to browser

Your API key never touches the browser. It lives only on Vercel's servers.
