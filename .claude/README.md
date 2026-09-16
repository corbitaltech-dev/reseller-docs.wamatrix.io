# WaMatrix Reseller docs — the `.claude/` layer

This folder holds the agents and skills that write, draft, and monitor the reseller-facing documentation at `reseller-docs.WaMatrix.io`. It mirrors the structure used by the sibling `docs.WaMatrix.io` project (see `/home/corbital/products-docs/docs.whatsmark.io/.claude/`), adapted for a completely different audience and product source.

**This is not the same audience as docs.WaMatrix.io.** That site writes for a small-business owner deciding whether to buy a chat tool — plain language, no jargon, no numbers outside one page. This site writes for a **reseller/agency owner operating a white-label WhatsApp SaaS** — an operator manual where "tenant", "quota", and real field-maximum numbers are correct, expected content. Do not import rules from that project's skills without checking `wamatrix-reseller-docs` first; several are deliberately inverted here.

---

## Layout

```
.claude/
├── agents/
│   ├── reseller-docs-writer.md      — general-purpose author: write, rewrite, review, fact-check any page
│   ├── reseller-docs-drafter.md     — turns short admin-screen notes + screenshots into a finished page
│   ├── reseller-docs-reviewer.md    — QA gate: pass/fail punch list on a changed page or PR
│   └── reseller-docs-manager.md     — MASTER agent: runs the full audit, auto-fixes what's safe, routes the rest
├── skills/
│   ├── wamatrix-reseller-docs/SKILL.md            — the rulebook: voice, brand, reseller/tenant terminology, structure, Mintlify + API-tab mechanics
│   ├── reseller-docs-fact-check/SKILL.md           — verify a claim against the real reseller panel source
│   ├── reseller-docs-competitor-benchmark/SKILL.md — benchmark against Wati/AiSensy/respond.io/Interakt/360dialog reseller programs
│   ├── reseller-docs-from-notes/
│   │   ├── SKILL.md                — method: short notes + screenshots → full on-brand page
│   │   └── INTAKE-TEMPLATE.md      — fill one per feature/screen; keeps notes structured
│   ├── reseller-docs-consistency-lint/SKILL.md     — brand, reseller/tenant terminology, gating language, images, icons, casing
│   ├── reseller-docs-grammar-style/SKILL.md        — proofreading pass, including field-reference tables
│   ├── reseller-docs-link-integrity/SKILL.md       — links, anchors, image paths, docs.json + OpenAPI validity, Meta soft-404s
│   └── reseller-docs-audit/
│       ├── SKILL.md                — full-site health sweep + report template + severity model
│       └── reports/                — dated audit reports land here
```

## How the two layers fit together

| Layer | Files | Job |
| --- | --- | --- |
| **Rulebook** | `wamatrix-reseller-docs` | The authoritative rules: voice, brand, reseller-vs-tenant terminology, gating language, Mintlify + API-tab mechanics, shared-repo safety, checklist |
| **Writing** | `reseller-docs-writer` + `reseller-docs-fact-check`, `reseller-docs-competitor-benchmark` | Author and fact-check pages from scratch |
| **Drafting from notes** | `reseller-docs-drafter` + `reseller-docs-from-notes` (+ `INTAKE-TEMPLATE.md`) | Turn short bullets + screenshots into a finished page |
| **Monitoring / QA** | `reseller-docs-manager` + `reseller-docs-reviewer` + the 4 monitoring skills | Keep the whole site healthy; gate every change |

Nothing is duplicated — every skill points back to `wamatrix-reseller-docs` as the single source of truth.

## The product source this project fact-checks against

**`/home/corbital/laravel/whatsmarkio_reseller`** — the actual reseller-panel Laravel/Vue/Inertia codebase. It ships its own deep `.claude/` layer (30+ feature-specialist agents and skills: `admin-crud`, `admin-api`, `payment-gateway`, `tenant-billing`, `whatsapp-channels`, `whatsapp-templates`, etc., plus a `.claude/memory/` of project decisions and gotchas). `reseller-docs-fact-check` is the map between a `pa/*.mdx` page and the matching screen/agent in that repo — read it before writing any factual claim.

This repo is a **white-label fork** of the upstream `whatsmark.io` consumer product (tracked via that repo's own `whatsmarkio-sync` agent) — never assume a whatsmark.io customer-facing behaviour applies here unmodified.

## How to use it

- **Draft a page from your notes:** fill `reseller-docs-from-notes/INTAKE-TEMPLATE.md` for the screen/feature, attach the screenshots, and hand both to `reseller-docs-drafter`.
- **Author a page from scratch:** ask `reseller-docs-writer`.
- **Before you publish / on a PR:** run `reseller-docs-reviewer` on the changed files.
- **Periodically, or after any reorg:** run `reseller-docs-manager` — it runs a full `reseller-docs-audit` sweep, auto-fixes the safe items on a scoped branch, and reports the rest.
- **One-off:** invoke any skill directly, e.g. "run reseller-docs-consistency-lint on `pa/`".

## The one rule that makes this safe

**Auto-fix only the unambiguous, mechanical things; escalate everything with a judgement, a fact, or a reseller/tenant terminology question.** The split is defined once, in `reseller-docs-manager` ("Decision model"), and every skill defers to it. The single highest-stakes category on this site — higher than on the customer-facing site — is **reseller vs. tenant/sub-tenant confusion**, because a swapped term in a billing or limits page can misconfigure real money flowing through a reseller's own business.

---

*Uses British spelling, matching the sibling `docs.WaMatrix.io` project's `.claude/` layer.*
