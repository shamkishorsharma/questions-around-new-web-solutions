---
name: new-web-solutions
description: AppsFlyer new web solution and Web Attribution Beta support workflow. Use when the user asks AppsFlyer customer questions about Web Attribution, website apps, Web SDK, S2S web attribution, web Data Locker reports, web-to-app or cross-platform attribution, CUID/cookie identity, traffic-source resolution, URL parameters, partner web integrations/postbacks, PBA migration, Support Engineer Slack ticket discussions, or asks for a customer-facing reply with AppsFlyer Support article links.
---

# New Web Solutions

## Overview

Use this skill to answer AppsFlyer Web Attribution / new web solution questions with the right balance of internal context and customer-safe wording. Treat Slack and Notion as internal evidence, but make final customer replies cite AppsFlyer Support articles whenever possible.

## Source Order

1. Start with `references/support-articles.md` for customer-safe links and official external wording.
2. Use the topic reference that matches the question:
   - Attribution, identity, windows, source resolution: `references/attribution-core.md`
   - Data Locker, tables, SQL, dashboards: `references/data-debugging.md`
   - Partner integrations and web postbacks: `references/partner-postbacks.md`
   - Known investigation patterns: `references/case-patterns.md`
3. Use `references/source-map.md` to reopen the Slack canvas, Notion cards, the Support Engineer ticket-discussion channel, or `#guru-web` fallback channel when the local references are not enough or the answer depends on current beta status, roadmap, pricing, availability, owners, live tickets, or recent behavior.
4. Use `references/customer-replies.md` before drafting the final customer-facing response.

## Tool Access

Use Slack and Notion connectors when available. If the current Claude environment does not have access to AppsFlyer Slack, Notion, or internal docs, do not invent live/current details; ask the user to paste the relevant thread, ticket, page, or screenshots, then answer from that material plus the bundled references.

## Customer-Safe Rules

- Do not share Slack, Notion, Jira, internal Google Docs, internal owners, internal uncertainty notes, raw SQL, or roadmap language with customers unless the user explicitly asks for an internal-only answer.
- If the answer needs a link, prefer `support.appsflyer.com` links. Internal sources can inform the answer but should not be linked in a customer reply.
- If a fact is beta status, GA timing, pricing, partner availability, "For users from" behavior, dashboard rollout, deprecation timing, or any ETA, re-check the live source before answering. If it is not confirmed externally, say that you are checking internally rather than promising it.
- If a customer question cannot be supported by a public KB article, give a cautious explanation and recommend confirming through CSM/Product/Support escalation instead of presenting internal assumptions as official policy.
- When the user asks for a customer-facing reply, produce a ready-to-send answer, not an internal analysis dump.

## Workflow

1. Classify the question: onboarding/setup, attribution logic, implementation, S2S, reporting/Data Locker, cross-platform, traffic source/URL params, partner postbacks, or known issue.
2. Load only the relevant reference files from `references/`.
3. Separate what is customer-safe from what is internal-only.
4. If the question is about current status or a recent Slack discussion, use the source map to fetch/search live Slack or Notion context before answering.
5. Draft the reply with a short diagnosis, practical next steps, and support article links.
6. Include clarifying questions only when needed to avoid a wrong answer, such as missing app ID, unified app ID, event name, date range, landing URL, CUID/cookie details, implementation method, or partner integration.

## Common Triggers

- "Why is web purchase organic?"
- "How does Web Attribution choose the media source?"
- "Can S2S work without the Web SDK?"
- "Why are web events pre-acquisition / unattributed?"
- "How do I match web events to conversions?"
- "Which Data Locker report/table should the customer use?"
- "Why are web postbacks not sent to Meta/Google/TikTok/Snap?"
- "How should I reply to this customer about Web Attribution Beta?"

## Reply Standard

Default to this shape for customer-facing replies:

1. Acknowledge the question and state the likely explanation.
2. Give a concise technical explanation using customer-safe terms.
3. List concrete checks or changes the customer can take.
4. Add "Useful references" with AppsFlyer Support links.
5. Avoid internal caveats unless phrased as "we are checking internally" or "we will confirm".
