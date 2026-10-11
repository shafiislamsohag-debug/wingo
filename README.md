# WinGo Big/Small Signal Dashboard (Starter)

React + Vite + Tailwind starter for a WinGo 30-second results dashboard.

## Included
- Dashboard overview and sample BIG/SMALL frequency analysis
- Result history, manual result entry, and CSV export
- User-supplied JSON results endpoint adapter
- Telegram test/send endpoints in a small Express server
- Clear demo/source status and non-guarantee notices

## Important limitations
- Initial records are illustrative demo data, **not live HGNice.bet results**.
- A frequency imbalance is descriptive only and does not establish that the next result is more likely to follow it.
- BIG/SMALL is implemented as 5–9 = BIG and 0–4 = SMALL. Confirm the exact game rules.
- The color mapping in this starter is only a placeholder and must be checked against the platform's official rules.
- “API connected” means an endpoint responded; it does not independently authenticate official results.
- Automatic scheduled Telegram signals are not enabled yet. They need a permitted, trusted results source and server-side scheduler.

## Run locally
Requires Node.js 18+.

```bash
npm install
cp .env.example .env
npm run dev
```

In a second terminal:
```bash
npm run server
```

Open the Vite URL shown in the first terminal (normally http://localhost:5173).

## Telegram
1. Create a Telegram bot with `@BotFather`.
2. Add it to the intended group and allow it to send messages.
3. Get the group/channel chat ID.
4. Start the backend with `npm run server`, then use the Telegram tab to test.
5. For production, **do not send/store the bot token in the frontend**. Change the backend to read `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` from environment secrets, authenticate dashboard users, add rate limits, and remove token fields from client requests.

The Vite proxy for `/api` is already configured in `vite.config.js` for local development. In production, route `/api` through your trusted reverse proxy to the backend.

## Connect result history
In Data source, provide an API URL that returns:
```json
[
  { "period": "202610110045", "value": 7 },
  { "period": "202610110044", "value": 2 }
]
```
or:
```json
{ "results": [{ "period": "202610110045", "value": 7 }] }
```
Only use a data source you are authorized to access. If the provider has no official API, use manual entry or implement a permitted server-side adapter. Do not bypass access controls or anti-bot protections.

## Next production steps
- Implement a source-specific, permitted server adapter after confirming the official result format.
- Store raw results, source timestamp, fetch timestamp, and verification status in a database.
- Reconcile every round by period ID and compare with the official game history.
- Add scheduled polling and Telegram delivery with duplicate-message protection.
- Backtest on out-of-sample results and report sample size and uncertainty; do not claim guaranteed wins.
