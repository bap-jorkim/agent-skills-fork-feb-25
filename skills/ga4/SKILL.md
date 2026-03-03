---
name: ga4
description: Query Google Analytics 4 (GA4) data via the Analytics Data API. Use when you need to pull website analytics like top pages, traffic sources, user counts, sessions, conversions, or any GA4 metrics/dimensions. Supports custom date ranges and filtering.
metadata:
  emoji: "📊"
---

# GA4 - Google Analytics 4 Data API

Query GA4 properties for analytics data: page views, sessions, users, traffic sources, conversions, and more.

## Setup (one-time)

1. Enable Google Analytics Data API: https://console.cloud.google.com/apis/library/analyticsdata.googleapis.com
2. Create OAuth credentials or use existing Google Cloud project
3. Set environment variables:
   - `GA4_PROPERTY_ID` - Your GA4 property ID (numeric, e.g., "123456789")
   - `GOOGLE_CLIENT_ID` - OAuth client ID
   - `GOOGLE_CLIENT_SECRET` - OAuth client secret
   - `GOOGLE_REFRESH_TOKEN` - OAuth refresh token (from initial auth flow)
4. Validate your setup:
   ```bash
   python3 scripts/ga4_query.py --metric sessions --dimension date --limit 1
   ```
   **Success:** Returns a one-row table with a date and session count.
   **Failure:** See Troubleshooting below.

## Common Queries

### Top Pages (by pageviews)
```bash
python3 scripts/ga4_query.py --metric screenPageViews --dimension pagePath --limit 30
```

### Top Pages with Sessions & Users
```bash
python3 scripts/ga4_query.py --metrics screenPageViews,sessions,totalUsers --dimension pagePath --limit 20
```

### Traffic Sources
```bash
python3 scripts/ga4_query.py --metric sessions --dimension sessionSource --limit 20
```

### Landing Pages
```bash
python3 scripts/ga4_query.py --metric sessions --dimension landingPage --limit 30
```

### Custom Date Range
```bash
python3 scripts/ga4_query.py --metric sessions --dimension pagePath --start 2026-01-01 --end 2026-01-15
```

### Filter by Page Path
```bash
python3 scripts/ga4_query.py --metric screenPageViews --dimension pagePath --filter "pagePath=~/blog/"
```

## Available Metrics

Common metrics: `screenPageViews`, `sessions`, `totalUsers`, `newUsers`, `activeUsers`, `bounceRate`, `averageSessionDuration`, `conversions`, `eventCount`

Full reference: https://developers.google.com/analytics/devguides/reporting/data/v1/api-schema#metrics

## Available Dimensions

Common dimensions: `pagePath`, `pageTitle`, `landingPage`, `sessionSource`, `sessionMedium`, `sessionCampaignName`, `country`, `city`, `deviceCategory`, `browser`, `date`

Full reference: https://developers.google.com/analytics/devguides/reporting/data/v1/api-schema#dimensions

## Output Formats

Default: Table format
Add `--json` for JSON output
Add `--csv` for CSV output

## Troubleshooting

- **Invalid refresh token / auth error:** Re-run the OAuth flow to obtain a new `GOOGLE_REFRESH_TOKEN` and update the environment variable.
- **Wrong property ID / property not found:** Confirm `GA4_PROPERTY_ID` is the numeric ID only (e.g., `123456789`), not the measurement ID (`G-XXXXXXXX`). Find it in GA4 under Admin → Property Settings.
