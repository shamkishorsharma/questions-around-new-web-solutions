# Attribution Core

## Product Framing

- A web campaign is defined by destination: if the user clicks an ad and lands on a website, it is a web campaign even if the ad was shown on Facebook, Google, TikTok, or another platform.
- Web Attribution is designed for direct-response click journeys where the marketing source can be observed on the landing URL.
- It is not designed to measure view-through impressions, multi-touch/contributor influence, or broad brand awareness unless a current public article says otherwise.
- AppsFlyer already supports web-to-app flows through Smart Script / Smart Banner. Web Attribution measures web-side conversions directly and supports web and cross-platform reporting where identity is available.

## Traffic Source Resolution

Resolve media source from the landing URL in this order:

1. AppsFlyer parameters such as `pid` / `af_` fields.
2. UTM parameters such as `utm_source`, `utm_medium`, and `utm_campaign`.
3. Network click IDs such as `gclid`, `gbraid`, `wbraid`, `fbclid`, `ttclid`, or `ScCid`.
4. HTTP referrer.
5. Direct / organic when no usable source is present.

Support implication: when a customer sees unexpected organic or fragmented media sources, ask for the exact landing URL and redirects. If the landing URL has no trackable parameter and no usable referrer, the visit is organic/direct from the attribution engine's perspective.

## Identity

- Web identity uses browser cookie plus CUID.
- Browser cookie / `appsflyer_id_value` is device and browser specific. It can expire or be blocked, and Safari has shorter cookie durability.
- CUID is the customer's stable user identifier passed at login/signup through the Web SDK or S2S API. It is the strongest signal and is required for reliable cross-device and cross-platform stitching.
- Cross-device attribution before login is fragile because there is no stable shared identifier.
- The source material describes a deliberate 30-minute attribution delay so the system can wait for many users to log in and pass CUID before finalizing attribution. No retroactive correction should be assumed after that window.

## Data Collection

Web SDK / Pixel calls:

- `LOAD`: fires on page load, records the visit, reads URL parameters, drops cookie.
- `IDENTIFY`: fires when the user logs in or is identified, passes CUID.
- `EVENT`: fires when the user completes an action.

S2S API:

- Sends visits, events, and identity directly from the customer's server.
- More resilient to browser restrictions and ad blockers.
- Requires correct identifiers. For S2S-only questions, verify the current Support article for accepted identifiers and whether the customer can provide first-party visitor identifiers without Web SDK extraction.

## Attribution Flows

- Visit attribution flow: runs on every visit/session and may create `FIRST_VISIT`, `REVISIT`, re-engagement, or re-acquisition conversion records.
- UA / re-acquisition flow: runs when the configured UA event or re-acquisition event fires. It chooses the acquisition anchor/source based on lookback and user state.
- Event attribution flow: runs for every web event and decides which existing attribution instance receives credit.

Common event outcomes:

- No history or no UA yet: `PRE_USER_ACQUISITION`.
- Active user, no retargeting: `USER_ACQUISITION`, primary.
- Active user with retargeting in window: dual attribution, retargeting primary and UA secondary.
- Churned/inactive user returning and triggering UA event: new `USER_ACQUISITION`.

## Settings And Windows

Source docs list common defaults such as 30-day visit/lookback windows, a 90-day inactivity window for re-acquisition, and a post-UA attribution window described as long-running / LTV-oriented. Always check the customer's actual app settings before making account-specific claims.

Important settings:

- UA event: `first_visit` by default or a custom event such as sign-up, registration, first purchase, first deposit, or subscription.
- Visit / lookback window: max time between non-organic visit and UA event.
- Attribution window: how long post-UA events continue to receive credit.
- Inactivity window for UA / re-acquisition: when a user is treated as churned and eligible for new acquisition.
- Re-engagement inactivity and re-engagement window: retargeting eligibility and credit duration.
- Excluded domains: internal, payment, OAuth, redirect, or self-referral domains that should not become attribution sources.

## Common Explanations

- Purchase appears organic: no trackable landing URL, missing/stripped URL parameters, cookie expired or blocked, CUID not passed, user changed browser/device, UA event happened outside lookback, or direct return.
- Returning user appears new: CUID missing or inconsistent, cookie-only identity failed, user crossed the inactivity window, or user first returned from another device before CUID was recorded.
- UA event is organic: no non-organic visit found inside lookback, source URL lacked valid parameters, or identity stitching linked the UA event to a different anonymous session.
- Same event appears twice: expected dual attribution during active retargeting. Filter `is_primary_attribution = TRUE` for most reporting.

## Customer-Safe Limitations

Mention limitations only when relevant and preferably with a Support article link:

- No view-through / impression attribution unless public docs say it is supported.
- No multi-touch/contributor reporting unless public docs say it is supported.
- Cookie-based identification is less reliable than CUID.
- Cross-device matching depends on CUID.
- Direct/organic traffic without trackable parameters cannot be attributed to a paid source.
