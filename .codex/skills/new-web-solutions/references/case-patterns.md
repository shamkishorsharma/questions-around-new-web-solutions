# Case Patterns

Use this file to recognize recurring beta support issues. Treat customer names and internal investigation details as internal context unless the user asks for internal analysis.

## Incorrect HTTP_REFERRER Attribution / OneLink

Signal:

- High-volume `match_type = 'HTTP_REFERRER'`.
- `media_source` is a redirect/self-referral domain such as a branded OneLink domain, OAuth, CDN, payment provider, or internal tool.
- Paid touchpoints exist nearby, but credit falls to a referrer domain.

Likely causes:

- Missing excluded domains.
- Redirect strips campaign parameters before the user lands on the website.
- OneLink `af_r` destination does not receive forwarded `pid` / UTM parameters.

Checks:

- Top HTTP referrer media sources for the app.
- App `excluded_domains`.
- Whether `http_referrer` contains `pid` or UTM while `event_url` is clean.
- Legacy PBA comparison, if relevant.

Customer-safe response:

- Explain that the website landing URL/referrer path appears to be turning the redirect domain into the source.
- Recommend adding self-referral/internal/payment/OAuth redirect domains to excluded domains where appropriate and preserving attribution parameters through redirects.
- Link: Traffic source resolution, URL attribution parameters, Brand OneLink with your domain.

## Media Source Fragmentation From UTM Setup

Signal:

- Media sources look like placements: `facebook_mobile_feed`, `instagram_reels`, `an`, `fb`, `ig`, etc.
- `match_type = 'UTM'`.
- Campaign may be populated but adset/ad fields are empty.

Likely cause:

- Customer uses granular macros such as `utm_source={{placement}}` or `utm_source={{site_source_name}}`.
- `utm_medium` is missing or not mapped.
- No AppsFlyer `pid` is present, so raw UTM values become the media source.

Recommended pattern:

```plain text
pid=facebook_int&af_campaign={{campaign.name}}&af_campaign_id={{campaign.id}}&af_adset={{adset.name}}&af_adset_id={{adset.id}}&af_ad={{ad.name}}&af_ad_id={{ad.id}}&af_sub1={{placement}}&utm_source=facebook_int&utm_medium=paid_social&utm_campaign={{campaign.name}}&utm_content={{placement}}
```

Customer-safe response:

- Explain that AppsFlyer resolves by URL priority, so `pid` should carry the intended media source while placement can be preserved in sub-parameters or UTM content.
- Link: Traffic source resolution and landing page URL attribution parameters.

## Low CUID Coverage And Missing Cross-Platform UA

Signal:

- High web traffic but few web user acquisitions in cross-platform dashboard.
- Low percentage of web conversions/sessions with CUID.

Likely cause:

- Cross-platform user attribution depends on CUID. Cookie-only web visits cannot be matched to the user's cross-platform identity.
- Users are not logged in at first visit or the Web SDK `setCustomerUserId` call fires too late.
- Edge case: one repeated/shared CUID can be suppressed if it floods volume; investigate if CUID coverage looks high but UA remains near zero.

Customer-safe response:

- Explain that CUID is needed for cross-platform matching and that cookie-only traffic is limited to browser/device context.
- Ask whether the low CUID rate matches the customer's login behavior.
- Recommend reviewing when `setCustomerUserId` fires and ensuring it sends a unique, stable, per-user value.
- Link: User-based cross-platform attribution and Web SDK CUID setup.

## Missing Revenue On `af_purchase`

Signal:

- `af_purchase` events exist but revenue fields are all zero.
- Revenue values appear inside `event_value` as `af_revenue`, `af_price`, or custom amount fields.
- Currency appears inside metadata instead of the top-level web revenue currency field.

Likely cause:

- Customer used mobile-style rich event payload fields instead of Web SDK top-level revenue fields.

Customer-safe response:

- Explain that Web SDK revenue must be sent in the documented revenue fields, not only inside event metadata.
- Tell the customer to send both amount and currency in the Web SDK's revenue parameters. Historical events generally should not be expected to reprocess.
- Link: Web SDK event implementation article.

## S2S Events Pre-Acquisition / Unattributed

Signal:

- In-app events are `PRE_USER_ACQUISITION`, `CUSTOM_EVENT`, `is_attributed = false`, often all from S2S.
- SDK sessions carry cookie only.
- S2S events carry CUID only.
- No event carries both cookie and CUID.

Likely cause:

- Identifier gap between SDK visit stream and S2S event stream. The user graph has no edge to connect cookie-keyed visits with CUID-keyed server events.

Fix options:

- Preferred: call `setCustomerUserId()` from the Web SDK when the user signs up/signs in so SDK events carry both cookie and CUID.
- Alternative: include the web cookie (`af_web_id` / web SDK user ID) in S2S payloads, if the customer's architecture supports it.

Customer-safe response:

- Explain that AppsFlyer needs a shared identifier between the visit and event streams.
- Recommend sending CUID through the Web SDK and/or sending the web cookie with S2S events.
- Link: Web SDK CUID setup, S2S request body, extracting Web SDK `af_user_id`.

## S2S-Only Identity Questions

If a customer asks whether they can generate their own visitor token without the Web SDK:

- First verify the current S2S user identification Support article.
- Safe baseline: Web Attribution can use CUID and first-party web identifiers; CUID improves accuracy and is required for cross-platform stitching.
- Do not invent a guarantee that any arbitrary visitor token will behave like the AppsFlyer web cookie unless public docs confirm it.
- If the public article is unclear, say you will confirm the recommended S2S-only identifier approach internally.
