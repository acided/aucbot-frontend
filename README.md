# AUC BOT — Telegram Mini App

**A Telegram Mini App for tracking the in-game marketplace of STALZONE (ex-Stalcraft): live lots, price history and price alerts delivered by a Telegram bot.**

Built on the official game API — access was granted by the game developers. The app tracked the whole marketplace: 100,000+ lots.

This repo is the React frontend. Backend (FastAPI, alerts worker, bot): [acided/aucbot-backend](https://github.com/acided/aucbot-backend) — architecture, data model and API are described there.

> **Status:** finished personal project; the public instance is offline.

## Features

- **Region select** on first launch: EU, RU, NA, SEA
- **Auction** — item search by Russian or English name, all active lots sorted by price
- **Artifact filters** — by quality (Common → Legendary) and minimum potential
- **Price history** — SVG chart of recent sales with min / max / average price
- **Alerts** — create a price alert right from an item (with the current quality and potential filters), list and delete alerts, jump from an alert back to its item with the filters restored
- **Clans** — search clans by name or tag, view tournament match history with W/D/L stats
- Tab state is kept in `App` when switching tabs, so the search and the open item survive navigation
- Dark theme, Montserrat, built for the Telegram mobile client

## Tech stack

- React 18, Vite 5
- Telegram WebApp SDK (`window.Telegram.WebApp`)
- Plain CSS with custom properties, inline SVG charts — no UI or chart libraries

## How it talks to the backend

Every request sends the Telegram `initData` string in `X-Init-Data`. The backend verifies its HMAC signature with the bot token, so the user identity always comes from Telegram. All game API calls go through the backend — the frontend never sees the API credentials.

## Running locally

```bash
cp .env.example .env    # set VITE_API_URL
npm install
npm run dev
```

| Variable | Description |
|---|---|
| `VITE_API_URL` | Backend base URL. Leave empty to use the dev proxy to `http://localhost:8000` |

Outside Telegram there is no `initData`, so every API request fails auth. To test the full flow, open the app through the bot: set the bot's Web App URL in [@BotFather](https://t.me/BotFather) to a public HTTPS address of the dev server (for example via a tunnel).

## Project structure

```
src/
  api/client.js         API client: headers, endpoints
  App.jsx               Region check, tab routing, shared auction state
  components/NavBar.jsx Tab navigation
  pages/
    RegionSelect.jsx    First-launch region picker
    AuctionPage.jsx     Search, lots, filters, price history chart
    AlertsPage.jsx      Alert list
    ClansPage.jsx       Clan search and match history
  index.css             Theme tokens and base styles
```
