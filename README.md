# HealthDaily Static App

This is a free, static, responsive web app prototype for a personal n8n health workflow.

## What it does

- Dashboard for fasting, calories, steps, sleep, and coach insights
- Fasting screen to start/end/save fasting windows
- Meal screen for food photo metadata and calorie entry
- Trends screen for weekly summaries
- Settings screen for your n8n production webhook URL
- No paid UI library
- No backend required in the app
- No Google/Fitbit/OpenAI credentials in the browser

## Recommended architecture

Browser static app
→ n8n Webhook
→ Google Sheets tabs:
  - HealthDaily
  - MealLog
  - FastingLog
→ Gmail daily summary

## Important security note

Do not put Google, Fitbit, OpenAI, or other API keys in this static web app.
The browser should only send simple events to n8n.
All sensitive API calls should happen inside n8n.

## n8n webhook contract

Example POST body:

```json
{
  "event_type": "meal_saved",
  "source": "healthdaily_static_app",
  "timestamp": "2026-05-23T20:00:00-04:00",
  "payload": {
    "meal_type": "Lunch",
    "estimated_calories": 680,
    "notes": "rice, chicken curry, salad"
  }
}
```

## Suggested n8n workflow

1. Webhook Trigger
2. Switch on `event_type`
3. For `meal_saved`: append row to `MealLog`
4. For `fasting_window_saved`: append/update `FastingLog`
5. For `send_summary`: read Google Sheets and send Gmail
6. Respond to Webhook with `{"ok": true}`

## Hosting

Recommended free options:
- GitHub Pages: simplest for a personal static app
- Cloudflare Pages: strong free option with unlimited static requests/bandwidth
- Netlify Free: simple drag-and-drop deploy
- Vercel Hobby: good for personal projects

For the lowest-risk free setup, use GitHub Pages or Cloudflare Pages.
