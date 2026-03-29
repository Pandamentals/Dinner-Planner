# 🍽️ Dinner Planner

A lightweight, mobile-friendly weekly dinner planning app built as a single HTML file. Powered by Supabase as the backend — no server, no framework, no build step.

Hosted on GitHub Pages. Works great on iPhone.

---

## Features

### 📚 Library
- Add dinners with a name, optional note or recipe link, and serving size
- Star favorites and filter by them
- Toggle skip to exclude a dinner from random generation
- Add and edit ingredients per dinner
- Inline editing and delete

### 📅 This Week
- Generate a random Mon–Fri plan from your non-skipped dinners
- Avoids repeating meals from the previous week
- Reorder days with ↑↓ buttons
- Swap any meal randomly 🔀 or pick manually ≡
- View ingredients per day inline 🧾
- Copy the full week to clipboard

### 🛒 Shopping List
- Auto-built from this week's meals that have ingredients
- Grouped by dinner with a tap-to-check interface
- Progress counter, uncheck all, copy list to clipboard

### 📖 History
- Every generated week is saved automatically
- Expandable cards showing what was planned and when

---

## Setup

This app requires a [Supabase](https://supabase.com) project with the following tables:

### `dinners`
| Column | Type | Notes |
|---|---|---|
| `id` | `uuid` | Primary key, default `gen_random_uuid()` |
| `created_at` | `timestamptz` | Default `now()` |
| `name` | `text` | Required |
| `note` | `text` | Nullable |
| `servings` | `int4` | Nullable |
| `is_favorite` | `bool` | Default `false` |
| `skip_generate` | `bool` | Default `false` |
| `ingredients` | `text[]` | Default `{}` |

### `last_week`
| Column | Type | Notes |
|---|---|---|
| `id` | `int4` | Primary key — insert one row with `id = 1` |
| `dinner_ids` | `uuid[]` | Default `{}` |

### `week_history`
| Column | Type | Notes |
|---|---|---|
| `id` | `uuid` | Primary key, default `gen_random_uuid()` |
| `created_at` | `timestamptz` | Default `now()` |
| `meals` | `jsonb` | Array of `{day, id, name, note}` |

> After creating the tables, insert a seed row into `last_week`:
> ```sql
> INSERT INTO last_week (id, dinner_ids) VALUES (1, '{}');
> ```

### Connecting
On first load the app shows a setup screen. Paste your **Project URL** and **anon public key** from Supabase → Project Settings → API. Credentials are saved in `localStorage` — one-time setup per device.

---

## Deployment

This is a single `index.html` file deployed via GitHub Pages.

```bash
# Make changes locally, then:
git add index.html
git commit -m "your message"
git push
```

GitHub Pages rebuilds in ~30–60 seconds.

---

## Stack

| Layer | Tech |
|---|---|
| Frontend | Vanilla HTML/CSS/JS — zero dependencies |
| Database | Supabase (Postgres via REST API) |
| Hosting | GitHub Pages |
| Fonts | Google Fonts — Playfair Display, DM Sans |
