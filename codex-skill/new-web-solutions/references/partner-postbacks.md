# Partner Integrations And Web Postbacks

Use this for partner setup, web campaign integrations, and optimization postback questions.

## Supported Web Partner PIDs

- Meta Web: `metaweb_int`
- Google Ads Web: `googleads_int`
- TikTok Web: `tiktokweb_int`
- Snapchat Web: `snapweb_int`
- Pinterest: listed as TBD in internal notes; verify current status before answering.

Some downstream reporting may consolidate web PIDs into main media sources:

- `metaweb_int` -> `facebook_int` / Facebook Ads
- `googleads_int` -> `googleadwords_int`
- `tiktokweb_int` -> `tiktokglobal_int`
- `snapweb_int` -> `snapchat_int`

From the customer's setup perspective, web integrations are configured separately.

## What Integrations Do

The web partner integrations primarily support:

- Partner setup/configuration for website apps.
- In-app event mapping.
- Optimization postbacks / conversion APIs.
- Cost and future partner infrastructure.

They are not the main source of web attribution engagements. Web source identification comes from the landing URL: AppsFlyer parameters, UTMs, click IDs, and referrer fallback.

## Key Behaviors

- Web signals consume final web attribution/event data, especially `end_user_events`.
- Only web IN_APP events are forwarded to networks.
- Website sessions / visits / `af_app_opened` are not sent as web postbacks.
- Page views can be sent only if implemented/mapped as IN_APP events.
- For web postback debugging, check integration active status, event mapping, platform = Website, required partner credentials, and required event identifiers.
- Internal notes conflict on "For users from" support: one page says only Meta supports both "This partner only" and "All media sources including organic" at launch, while another says Meta, Snap, and TikTok support both and Google supports only "This partner only". Verify current Support KB / product status before making a customer-facing claim.

## Minimum Requirements By Partner

Meta Web (`metaweb_int`):

- Dashboard: Pixel ID and Access Token.
- Event needs at least one of: `fbclid`, Custom User ID, IP address, or User Agent.
- Optional: event URL, revenue, currency.

Google Ads Web (`googleads_int`):

- Dashboard: Customer ID, MCC Customer ID, Google access token, Google developer token, mapped conversion action.
- Event needs Customer ID plus at least one of: `gclid`, `gbraid`, or `wbraid`.
- Optional: revenue and currency.

Snapchat Web (`snapweb_int`):

- Dashboard: Pixel ID and Access Token.
- Event needs at least one of: IP address or User Agent.
- Optional: Snap click ID (`ScCid`), Custom User ID, event URL, revenue, currency.

TikTok Web (`tiktokweb_int`):

- Dashboard: Pixel ID and Access Token.
- Event needs at least one of: IP address, User Agent, or Custom User ID.
- Optional: `ttclid`, event URL, revenue, currency.

## Debugging Checklist

- No postbacks for any web partner: integration active? event mapped? event is IN_APP and platform Website? not a session?
- No Meta postback: Pixel ID and token set? event has `fbclid`, Custom User ID, IP, or User Agent?
- No Google postback: Customer ID and auth set? event has `gclid`, `gbraid`, or `wbraid`? conversion action mapped?
- No Snap postback: Pixel ID/token set? event has IP or User Agent?
- No TikTok postback: Pixel ID/token set? event has IP, User Agent, or Custom User ID?
- Partner rejection: token expired, wrong customer/pixel ID, or missing/wrong conversion mapping.
- Click ID missing: check generic `clickid` and `clickid_type`, not only partner-specific field names.

## Customer-Safe Links

Use partner-specific Support articles from `support-articles.md`:

- Google Ads web campaigns.
- Meta Ads web campaigns.
- TikTok for Business web campaigns.
- Snapchat web campaigns.
