# New Web Solutions

An AppsFlyer support skill for answering questions about Web Attribution and related web solutions. It helps produce technically grounded, customer-safe responses while keeping internal context separate from externally shareable guidance.

> **Internal use only:** The bundled references may contain internal AppsFlyer context. Review generated answers before sharing them externally and use public AppsFlyer Support links for customer-facing claims.

## What the skill covers

- Web Attribution and website app onboarding
- Web SDK and server-to-server implementations
- Attribution logic, identity, cookies, and CUID
- Traffic-source resolution and URL parameters
- Web Data Locker reports and debugging
- Web-to-app and cross-platform attribution
- Partner integrations and web postbacks
- PBA migration and known support investigation patterns
- Customer-facing replies with AppsFlyer Support references

## Repository layout

```text
.
├── .codex/skills/new-web-solutions/       # Workspace-ready Codex skill
├── codex-skill/
│   ├── new-web-solutions/                 # Portable Codex skill folder
│   └── new-web-solutions-codex-skill.zip  # Packaged Codex distribution
└── claude-skill/
    ├── new-web-solutions/                 # Portable Claude skill folder
    └── new-web-solutions-claude-skill.zip # Packaged Claude distribution
```

Each skill folder contains `SKILL.md` and focused references for attribution, reporting, partner postbacks, investigation patterns, source discovery, and customer replies. The Codex package also includes `agents/openai.yaml` for its display metadata and default prompt.

## Use in this Codex workspace

Clone or open the repository as a Codex workspace. Codex can discover the skill from:

```text
.codex/skills/new-web-solutions/SKILL.md
```

Invoke it explicitly with:

```text
Use $new-web-solutions to answer this AppsFlyer Web Attribution customer question.
```

The skill can also activate automatically when a request matches its description.

## Install the portable Codex package

Copy `codex-skill/new-web-solutions/` into your Codex skills directory so the resulting path is:

```text
~/.codex/skills/new-web-solutions/SKILL.md
```

Alternatively, extract `codex-skill/new-web-solutions-codex-skill.zip` into the same skills directory. Restart or reload Codex if the skill is not detected immediately.

## Install the Claude package

Import or copy `claude-skill/new-web-solutions/` into the skills location supported by your Claude environment. The prebuilt archive is available at:

```text
claude-skill/new-web-solutions-claude-skill.zip
```

Ensure the installed folder preserves `SKILL.md` and the complete `references/` directory.

## Example requests

- Why is this web purchase attributed as organic?
- Can Web Attribution work through S2S without the Web SDK?
- Which Data Locker report should the customer use for web events?
- Why was a web postback not sent to the partner?
- Draft a customer-safe reply about Web Attribution Beta with public support links.

## Response principles

The skill is designed to:

1. Prefer official AppsFlyer Support articles for customer-facing wording.
2. Load only the references relevant to the question.
3. Separate internal evidence from externally shareable information.
4. Re-check live sources for changing details such as beta status, pricing, availability, rollout timing, and roadmap items.
5. Return concise explanations, practical checks, and useful links.

