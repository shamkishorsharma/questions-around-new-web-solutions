# Source Map

Use this file to find and refresh source context. Treat Slack, Notion, Jira, Google Docs, and channel history as internal-only unless the user explicitly asks for an internal answer.

## Primary Internal Sources

- Slack canvas link hub: https://appsflyer.enterprise.slack.com/docs/T02B3AJ9B/F0B8QVDFUAH
  - Canvas ID for Slack tool: `F0B8QVDFUAH`
  - Contains onboarding steps, training links, public KB links, and tools.
- Notion root: https://app.notion.com/p/appsflyerrnd/Web-Attribution-Beta-31ab7b13af5f80c69252f2b0e2288c27
  - Root page title: `Web Attribution [Beta]`
  - Contains child cards for overview, handling, domain mapping, tables, integrations, signals, FAQs, architecture, data collection, event attribution flow, and case studies.
- Support Engineer global ticket-discussion Slack channel: https://appsflyer.enterprise.slack.com/archives/C0B8KP6TXGX
  - Channel ID: `C0B8KP6TXGX`
  - Use this to refresh context from real Support Engineer ticket threads and questions across global teams.
  - Search this before `#guru-web` when the user asks about a customer ticket, a recent support question, or how Support has been handling a similar issue.
- Fallback Slack channel: `#guru-web`
  - Channel ID: `CDXM2DBD5`
  - Link: https://appsflyer.enterprise.slack.com/archives/CDXM2DBD5
  - Use only when the curated Slack canvas and Notion pages do not answer the question or the issue is recent.

## Notion Cards To Fetch By Topic

- Product and support overview: https://app.notion.com/p/320b7b13af5f81de9c3cd6c2d02ba039
- Changes and support handling: https://app.notion.com/p/374b7b13af5f809b9f59c9aa4ea93ae9
- Domain ownership / routing: https://app.notion.com/p/2dbb7b13af5f820ea3fe011e8a713524
- Data tables and query rules: https://app.notion.com/p/350b7b13af5f80f18061e761522a4d0e
- Partner integrations: https://app.notion.com/p/333b7b13af5f809eb632dfabae8dabfa
- Signals / postbacks: https://app.notion.com/p/36bb7b13af5f802d9a48c638c0061173
- FAQs: https://app.notion.com/p/324b7b13af5f80aa905ee2eaf9a5cb7c
- R&D reference index: https://app.notion.com/p/37eb7b13af5f83b4b488819e92418121
- Architecture / attribution engine: https://app.notion.com/p/30ab7b13af5f80928ae6c8f93aadca47
- Data collection: https://app.notion.com/p/37ab7b13af5f80cf9e0dfbab0b036291
- Event attribution flow: https://app.notion.com/p/382b7b13af5f81daa173ca21208f8b25
- Web postback requirements: https://app.notion.com/p/373b7b13af5f80b18661e309df991a33

## Case Study Cards

- Incorrect HTTP_REFERRER attribution / OneLink forwarding: https://app.notion.com/p/350b7b13af5f804186dff4024adcbd63
- Media source fragmentation from UTM setup: https://app.notion.com/p/350b7b13af5f80fc981ccdb0193f8b4e
- Low CUID coverage and missing cross-platform web UA: https://app.notion.com/p/386b7b13af5f81af8470d31189222d31
- Missing revenue on `af_purchase`: https://app.notion.com/p/386b7b13af5f81ad9957e1c269ff9583
- S2S in-apps pre-acquisition from cookie/CUID mismatch: https://app.notion.com/p/386b7b13af5f81579b79de1a889f8b16

## Slack Search Patterns

Use public Slack search in the Support Engineer ticket-discussion channel and `#guru-web` unless the user explicitly authorizes broader private search. If the Slack connector cannot resolve the Support Engineer channel name, search by channel ID with `in:<#C0B8KP6TXGX>`.

- Support tickets by topic: `"Web Attribution" in:<#C0B8KP6TXGX>`
- Support S2S identity: `"S2S" "user identification" in:<#C0B8KP6TXGX>`
- Support CUID/cookie: `"CUID" "cookie" in:<#C0B8KP6TXGX>`
- Support Data Locker: `"Data Locker" "web attribution" in:<#C0B8KP6TXGX>`
- Support partner postbacks: `"postback" "web" in:<#C0B8KP6TXGX>`
- Guru exact topic: `"Web Attribution" in:#guru-web`
- Guru S2S identity: `"S2S" "user identification" in:#guru-web`
- Guru CUID/cookie: `"CUID" "cookie" in:#guru-web`
- Guru Data Locker: `"Data Locker" "web attribution" in:#guru-web`
- Guru PBA migration: `"PBA" "web attribution" in:#guru-web`
- Guru partner postbacks: `"postback" "web" in:#guru-web`
- Guru current status: `"open beta" "web attribution" in:#guru-web`

## Current-State Rule

Do not rely on cached notes for beta/open beta/GA, pricing, availability, dashboard rollout, deprecation, partner support, or roadmap. Search live Slack/Notion and, when possible, the public Support KB before answering.
