# Kian's Personal Dashboard

A mobile-first personal dashboard built as a single-page web app — no frameworks, no build step, just HTML + CSS + JS deployed on Vercel.

**🔗 Live:** [ytd-ashboard.vercel.app](https://ytd-ashboard.vercel.app)

---

## Tabs

| Tab | What it does |
|---|---|
| **Dashboard** | Daily goals tracker, completion ring, goal ticker |
| **Health** | WHOOP live data (recovery ring, sleep stages, HRV, strain, biomarkers, today's verdict) + supplement stack + water coach |
| **Finance** | Net worth by category, subscription tracker, order tracking, wishlist |
| **Gym** | Workout logger with exercise history |

---

## Features

- **WHOOP integration** — OAuth 2.0 connect flow, live recovery score with animated ring, sleep stage breakdown (REM/Deep/Light/Awake), biomarkers (skin temp, SpO₂, resp rate), and a daily GREEN/YELLOW/RED training verdict
- **Supplement tracker** — morning/lunch/evening windows, taken/skipped logging, cross-device sync
- **Water coach** — bottle/glass tracking with 7-day history sparkline, personalized targets, and smart reminders
- **Finance tracker** — net worth across bank/stocks/crypto/other, monthly subscription totals, order status tracking, prioritized wishlist
- **Persistent top bar** — live GOALS / STACK / WATER counts on every page with a quick +1 water button
- **Cross-device sync** — Supabase realtime keeps all tabs in sync across devices
- **Offline-first** — localStorage as primary store, Supabase as sync layer
- **Mobile-optimized** — safe-area insets, iOS PWA-ready, no zoom, smooth 60fps animations

---

## Tech Stack

- **Frontend** — Vanilla HTML/CSS/JS (zero dependencies, zero build step)
- **Backend** — Vercel serverless functions (Node.js ESM)
- **Database/Sync** — [Supabase](https://supabase.com) (Postgres + realtime)
- **Wearable API** — [WHOOP Developer API](https://developer.whoop.com) v1/v2
- **Hosting** — [Vercel](https://vercel.com)

---

## Project Structure

```
├── index.html              # Dashboard / Goals tab
├── health.html             # Health tab (WHOOP + supplements + water)
├── gym.html                # Gym tab
├── finance.html            # Finance tab
├── topbar.js               # Shared sticky top bar (injected on every page)
└── api/
    ├── whoop-callback.js   # OAuth code → token exchange
    ├── whoop-refresh.js    # Silent token refresh proxy
    └── whoop-data.js       # WHOOP API proxy (routes v1/v2 correctly)
```

---

## Setup & Deploy

### 1. Clone
```bash
git clone https://github.com/kianc-droid/YTDashboard.git
cd YTDashboard
```

### 2. WHOOP Developer App
1. Go to [developer.whoop.com](https://developer.whoop.com) → create an app
2. Set the **Redirect URI** to `https://YOUR-VERCEL-DOMAIN.vercel.app/api/whoop-callback`
3. Copy your **Client ID** and **Client Secret**

### 3. Supabase
1. Create a project at [supabase.com](https://supabase.com)
2. Create a table called `app_state` with columns: `key` (text, primary key), `state` (jsonb), `updated_at` (timestamptz)
3. Copy your project URL and publishable (anon) key

### 4. Environment Variables
Add these to Vercel → Settings → Environment Variables:

| Variable | Value |
|---|---|
| `WHOOP_CLIENT_ID` | Your WHOOP app Client ID |
| `WHOOP_CLIENT_SECRET` | Your WHOOP app Client Secret |
| `WHOOP_REDIRECT_URI` | `https://YOUR-DOMAIN.vercel.app/api/whoop-callback` |

> ⚠️ The Client Secret never goes in the frontend code — it lives only in Vercel env vars.

### 5. Deploy
```bash
vercel --prod
```

### 6. Update hardcoded values
In each HTML file and `topbar.js`, replace:
- `SUPABASE_URL` → your Supabase project URL
- `SUPABASE_KEY` → your Supabase anon/publishable key
- `CLIENT_ID` in `health.html` → your WHOOP Client ID

---

## Local Development

Since there's no build step, you can open the HTML files directly in a browser for UI work. For the WHOOP API routes (which need server-side env vars), run locally with:

```bash
vercel dev
```

---

## License

MIT — use it, fork it, make it your own.
