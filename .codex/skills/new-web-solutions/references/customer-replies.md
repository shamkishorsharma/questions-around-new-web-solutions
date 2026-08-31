# Customer Replies

Use this before drafting a customer-facing answer.

## Style

- Be clear, practical, and calm.
- Keep internal uncertainty out of the customer reply unless the right answer is to confirm internally.
- Avoid "beta roadmap" promises, dates, or unsupported commitments.
- Use "based on the implementation/data pattern" rather than "bug" unless confirmed.
- Prefer "we recommend" and "please check" over heavy blame language.

## Default Reply Template

```text
Hi [Name],

Thanks for the details. Based on what you described, the likely reason is [short explanation].

In Web Attribution, [plain-English product behavior]. This means [customer impact].

Recommended checks:
1. [Check or action]
2. [Check or action]
3. [Check or action]

Useful references:
- [Support article title](URL)
- [Support article title](URL)

Best,
[Name]
```

## If The Answer Is Not Fully Confirmed

```text
Hi [Name],

Thanks for raising this. The expected behavior depends on [specific condition], so I am going to confirm this internally before giving you a definitive answer.

In the meantime, the relevant documented setup is here:
- [Support article title](URL)
```

## Common Snippets

New Web Attribution reporting:

```text
For customers using the new Web Attribution solution, standalone website metrics are available under My Dashboards > Templates > Web Analytics. This is the primary dashboard for analyzing web performance independently; the legacy PBA dashboard is not required. For individual event rows, complete event payloads, or HTTP-referrer analysis, use the Web End User Events report in Data Locker. Use the Cross-Platform dashboard when the goal is to analyze CUID-based journeys across web and other apps in the same Product Line.
```

Organic / direct:

```text
For a web visit to be attributed to a paid source, the landing URL needs to carry a valid attribution signal, such as AppsFlyer parameters, UTM parameters, a supported click ID, or a usable referrer. If none of these are present after redirects, the visit is treated as organic/direct.
```

CUID / cross-platform:

```text
For cross-platform attribution, CUID is the key identifier. Cookie-only web traffic can help identify activity within the same browser/device, but CUID is what enables stable user-level matching across web and app journeys.
```

SDK + S2S identity mismatch:

```text
The visit and event streams need at least one shared identifier. If Web SDK visits include only the web cookie while S2S events include only CUID, AppsFlyer cannot reliably connect those records. The recommended fix is to pass CUID through the Web SDK when the user is identified, or include the web cookie in the S2S event payload where applicable.
```

Revenue:

```text
For Web SDK purchase events, revenue must be sent in the Web SDK revenue fields. If the amount is sent only inside generic event metadata, the event can still be recorded, but revenue reporting may remain zero.
```

UTM fragmentation:

```text
The media source is resolved from the landing URL. If `utm_source` contains placement-level macros, each placement can appear as a separate media source. We recommend using the AppsFlyer `pid` for the intended network/media source and storing placement in a sub-parameter or content field.
```

Postbacks:

```text
For website apps, partner postbacks are sent for mapped in-app events, not for sessions/visits. Please confirm that the web partner integration is active, the event is mapped, and the event contains the required partner identifiers.
```

## Link Selection

- Always include 1-3 links, not a giant list.
- Link exact docs for the action requested: SDK for implementation, S2S for server-side, Data Locker for raw reports, traffic-source docs for URL/source questions, partner docs for postbacks.
- If the public KB does not support the claim, do not link internal materials; phrase the answer cautiously and say you will confirm internally.
