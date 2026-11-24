# Counter

Independent cafés in Singapore price by feel. They check a competitor's Instagram, guess, and change nothing for months. Counter shows an owner their menu next to the cafés around them: who is cheaper on a flat white, who just launched a new pastry, who is running an offer this week, and what the owner could do about it.

- Landing page: https://countr.site
- App: https://app.countr.site

We built it as a student team (NUS, TUM, Universitas Indonesia and UofT) for ETP4212, Venture Creation, at NUS in autumn 2026. Between August and October we read nearly 6,000 prices from 96 cafés and bakeries, shop by shop. Arshin Sikka built the first version of the app, and his commits are kept in this history. v2 adds a new first screen, opens the app without an access code and adds price alerts. `CHANGES.md` lists every change.

## What's in it

- `menus/`: 96 Singapore café and bakery menus, one JSON file per shop, with item names and prices.
- `counter_data.json`: shop locations and the competitor sets used for comparisons.
- `index.html`: the whole app. Owners pick their shop and their competitors and get a price comparison, recent launches, current offers and ideas for their own offers.
- `api/read-menu.js`: the one serverless function. It reads a menu that only exists as a photo or a scanned PDF.

## Where a model is used

Only in one place: reading a menu from a **photo or a scanned PDF**, through OpenRouter.

Everything else runs without one:

- **A PDF menu with real text** is read in the browser with pdf.js. That is exact and free. The model is only called if fewer than 5 priced items come back.
- **A file uploaded twice** is not read twice. The last 5 reads are kept in the browser.
- **Ideas for your shop** are rules on the owner's menu, the usual prices nearby and the offers nearby (`makeIdeas` in `index.html`).

## Run it

1. Put the OpenRouter key in `.env` as `OPENROUTER_API_KEY=...`. The file is git-ignored.
2. Copy it to Vercel with `vercel env add OPENROUTER_API_KEY production`, then `vercel deploy --prod`.
3. Set a credit limit on the key at openrouter.ai. There is no access code, so the key limit is what caps spending.

The Supabase key in `index.html` is the public anon key, which is meant to ship to the browser.
