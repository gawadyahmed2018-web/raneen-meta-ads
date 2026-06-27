# Raneen Meta Ads Dashboard — Standalone

A **standalone** Streamlit dashboard sourced **directly from Meta (Facebook/Instagram)
Ads via Windsor.ai** — independent from the GA4-based `raneen-analytics` dashboard.

## Why a separate dashboard?

The GA4 dashboard (`raneen-analytics`) measures Meta campaign performance through
**UTM parameters and GA4 session attribution**. This creates a well-known and
expected gap vs. Meta Ads Manager numbers (e.g. Meta shows 403 purchases for a
campaign, GA4 shows 41) due to:

- iOS 14.5+ App Tracking Transparency — many purchases never reach GA4's
  client-side tracking
- Facebook/Instagram in-app browser sessions getting lost or misattributed
- Meta's wider attribution window (up to 7-day click) vs GA4's session-based model
- Meta's modeled/estimated conversions for tracking-restricted users

**This dashboard solves that** by pulling numbers **straight from Meta itself**
(via the Windsor.ai Facebook Ads connector), so Spend, Purchases, and ROAS here
should match Meta Ads Manager almost exactly.

## Features

- **Overview KPIs**: Spend, Purchases, Purchase Value, ROAS, Impressions, Clicks, CTR, CPC, CPA
- **Reporting level switch**: Campaign / Ad Set / Ad
- **Spend vs Purchases** trend chart
- **Purchase Value** trend (area chart)
- **Full performance table** with Rating badges (قوي / متوسط / ضعيف)
- **Best ROAS / Needs Review** quick-glance charts
- **CSV export**
- **Built-in Debug panel** — shows exactly which Windsor fields returned data,
  so any field-name mismatch is caught immediately instead of silently showing zeros

## Setup

```bash
pip install -r requirements.txt
streamlit run app.py
```

## Deploy on Streamlit Cloud

1. Push this repo to GitHub
2. [share.streamlit.io](https://share.streamlit.io) → New app
3. Repository: this repo · Branch: `main` · Main file: `app.py`
4. Deploy

## Field names note

Windsor's Facebook Ads connector supports 560+ metrics. This dashboard tries the
documented purchase-tracking fields first (`actions_purchase`, `action_values_purchase`,
`purchase_roas`), and **automatically falls back** to alternate names
(`purchases`, `purchase_value`, `roas`) if the primary ones return no data.

Open the **"🔍 Debug"** expander at the top of the dashboard after first deploy —
it shows the exact columns returned by Windsor for your account. If purchases/ROAS
show as 0 even with ad spend present, check the Debug panel column list and adjust
`ACTION_FIELDS` / `ALT_ACTION_FIELDS` in `app.py` to match what's actually available
on your Meta connector setup.

## Stack

- **Streamlit** — UI
- **Plotly** — charts
- **Windsor.ai** — Facebook/Meta Ads connector (`https://connectors.windsor.ai/facebook`)
- **Pandas** — data processing
