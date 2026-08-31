# Data And Debugging

This reference is for internal investigation. Do not paste SQL or internal table names into a customer-facing reply unless the user explicitly wants an internal Support/TPE answer.

## Primary Production Tables

- `af-ldl-prd.attribution_end_user_events.end_user_events`
  - Default table for sessions, visits, in-app events, event-level analysis, funnel analysis, and raw event troubleshooting.
  - Mirrors final customer-facing attribution logic for granular events.
- `af-ldl-prd.attribution_conversions.conversions`
  - Source of truth for conversion anchors: first visits, revisits, user acquisition, re-acquisition, retargeting, and cohort/LTV anchors.
- `af-ldl-prd.apps.apps`
  - App configuration, attribution windows, excluded domains, timezone/currency, Web SDK ID, and app metadata.

## Pipeline / Debug Tables

- `datalake.pba_enriched`: legacy PBA validation and volume comparison.
- `datalake.web_events_invalid`: rejected events; raw JSON in `value`; parse with JSON functions.
- `datalake.web_events_enriched`: valid AWS-side enriched web events after URL parameter resolution.
- `dl_web_events_enriched` in GCP BigQuery: copied valid events in GCP.
- `web_events_intermediate`: events after CUID enrichment and before the attribution engine.

## Query Rules

- Put `event_time` filtering first in `WHERE`. Default to last 7 days if the time range is not specified.
- Add `platform = 'WEBSITE'` unless cross-platform analysis is explicitly requested.
- In `end_user_events`, add `is_primary_attribution = TRUE` by default to avoid double-counting dual attribution.
- Remove the primary filter only when specifically analyzing retargeting or secondary UA credit.
- Use `conversions` for counts of first visits, revisits, user acquisitions, re-acquisitions, and retargeting conversions.
- Use `end_user_events` for total sessions/events and event-level details.
- Use `end_user_event_type = 'SESSION'` for sessions/visits.
- Use `end_user_event_type = 'IN_APP'` when filtering by event names.
- Use `is_organic` to distinguish organic and paid. Do not infer from `media_source` text.
- For distinct users within `end_user_events`, prefer `user_counting_key` when available. For cross-table logic, use `COALESCE(customer_user_id, appsflyer_id_value)`.
- In `conversions`, `user_counting_key` is not available. Use `COALESCE(customer_user_id, appsflyer_id_value)` for joins or distinct user approximations.
- For revenue, use `revenue_usd > 0` or the relevant revenue field `> 0`, not `IS NOT NULL`.

## Counting Clicks

On web, clicks are inferred from non-organic entries to the site. Query `conversions`:

```sql
WHERE is_organic = FALSE
  AND conversion_name IN ('FIRST_VISIT', 'REVISIT')
```

## Useful Query Skeletons

User event timeline:

```sql
SELECT *
FROM `af-ldl-prd.attribution_end_user_events.end_user_events`
WHERE event_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 20 DAY)
  AND platform = 'WEBSITE'
  AND is_primary_attribution = TRUE
  AND user_counting_key = '<user_counting_key>'
ORDER BY event_time ASC
```

Find `user_counting_key` from CUID:

```sql
SELECT DISTINCT user_counting_key, appsflyer_id_value
FROM `af-ldl-prd.attribution_end_user_events.end_user_events`
WHERE event_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 30 DAY)
  AND platform = 'WEBSITE'
  AND customer_user_id = '<cuid>'
LIMIT 10
```

Organic vs paid split:

```sql
SELECT
  is_organic,
  COUNT(*) AS count,
  ROUND(COUNT(*) / SUM(COUNT(*)) OVER() * 100, 2) AS percentage
FROM `af-ldl-prd.attribution_end_user_events.end_user_events`
WHERE event_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
  AND platform = 'WEBSITE'
  AND is_primary_attribution = TRUE
  AND unified_app_id = '<unified_app_id>'
GROUP BY 1
ORDER BY 2 DESC
```

Conversion distribution:

```sql
SELECT
  conversion_name,
  conversion_type,
  COUNT(*) AS count
FROM `af-ldl-prd.attribution_conversions.conversions`
WHERE event_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
  AND platform = 'WEBSITE'
  AND unified_app_id = '<unified_app_id>'
GROUP BY 1, 2
ORDER BY 3 DESC
```

Identifier coverage by stream:

```sql
SELECT
  end_user_event_type,
  event_source,
  COUNT(*) AS cnt,
  COUNTIF(appsflyer_id_value != '') AS has_cookie,
  COUNTIF(customer_user_id != '') AS has_cuid,
  COUNTIF(appsflyer_id_value != '' AND customer_user_id != '') AS has_both
FROM `af-ldl-prd.attribution_end_user_events.end_user_events`
WHERE event_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
  AND platform = 'WEBSITE'
  AND is_primary_attribution = TRUE
  AND unified_app_id = '<unified_app_id>'
GROUP BY 1, 2
ORDER BY cnt DESC
```

## Customer Reply Translation

Translate internal findings into customer-safe language:

- "The event stream exists, but the identifiers do not let us connect visits and server events yet."
- "The source URL does not include a valid attribution signal, so the visit is treated as organic/direct."
- "The same event can appear twice when retargeting and original acquisition both receive credit; for primary reporting, use the primary attribution view."
- "Revenue is received only when sent in the Web SDK's revenue fields, not as generic event metadata."
