# 🍽️ Dinner Planner

A shared weekly dinner planner for two. Add dinners to a library, generate a Mon–Fri plan, manage a shopping list, and browse past weeks — all from a single hosted webpage with no app to install.

## Features

- **Library** — store dinners with a name, recipe link or note, serving size, and ingredients
- **This Week** — generate a random Mon–Fri plan that avoids last week’s picks, swap any meal randomly or pick manually
- **Shopping List** — ingredients grouped by meal with checkboxes to tick off while shopping
- **History** — every generated week saved by date for reference
- **Favorites** — star dinners to filter and highlight them
- **Skip toggle** — exclude dinners from generation without deleting them
- **Search** — live filter in the library
- **Shared** — both users see the same data in real time via Supabase

## Stack

|Layer   |Tech                                       |
|--------|-------------------------------------------|
|Hosting |GitHub Pages                               |
|Frontend|Single HTML file (vanilla JS, no framework)|
|Database|Supabase (PostgreSQL)                      |

## Setup

### 1. Supabase

1. Create a free account at [supabase.com](https://supabase.com)
1. Create a new project
1. Open the **SQL Editor** and run the following:

```sql
CREATE TABLE dinners (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  name text NOT NULL,
  note text,
  servings integer DEFAULT null,
  is_favorite boolean DEFAULT false,
  skip_generate boolean DEFAULT false,
  ingredients jsonb DEFAULT '[]',
  created_at timestamptz DEFAULT now()
);

CREATE TABLE last_week (
  id integer PRIMARY KEY DEFAULT 1,
  dinner_ids uuid[] DEFAULT '{}'
);

CREATE TABLE week_history (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  created_at timestamptz DEFAULT now(),
  meals jsonb NOT NULL
);

INSERT INTO last_week (id) VALUES (1) ON CONFLICT DO NOTHING;

ALTER TABLE dinners ENABLE ROW LEVEL SECURITY;
ALTER TABLE last_week ENABLE ROW LEVEL SECURITY;
ALTER TABLE week_history ENABLE ROW LEVEL SECURITY;

CREATE POLICY "public_all" ON dinners FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "public_all" ON last_week FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "public_all" ON week_history FOR ALL USING (true) WITH CHECK (true);
```

1. Go to **Project Settings → API** and copy your **Project URL** and **anon public** key

### 2. GitHub Pages

1. Fork or clone this repo
1. Go to **Settings → Pages → Branch: main → Save**
1. Visit `https://yourusername.github.io/dinner-planner`

### 3. First Launch

On first open the app shows a setup screen. Paste your Supabase Project URL and anon key and hit **Connect & Launch**. Keys are saved in the browser so you only do this once per device.

Anyone you share the URL with will need to go through the same one-time setup step with the same keys.

## Usage Tips

- **Ingredients** — open any dinner card, click *+ Add Ingredients*, type or paste a list (multiple lines supported for bulk add)
- **Skip toggle** — use this for meals you want a break from without removing them permanently
- **Shopping list** — checks are saved per device so two people can shop independently
- **Reverting** — every deploy is a GitHub commit, use the file history to roll back if needed

## Versioning

Each update is committed with a descriptive message. To revert: open `index.html` in the repo → click **History** → select a previous version → copy contents → paste back and commit.
