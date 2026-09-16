---
name: reseller-docs-competitor-benchmark
description: Method for benchmarking reseller-docs.WaMatrix.io pages against competing white-label / reseller WhatsApp SaaS platforms to find operator-onboarding patterns worth copying and the honest differentiators WaMatrix.io's reseller program can claim that they cannot. Activate before writing or rewriting the quick start, a plans/billing setup page, or any page meant to help a prospective reseller evaluate the platform; when asked how a competitor's white-label program documents something; or when a page reads generic and needs a sharper, more operator-specific angle.
---

# Benchmarking the reseller docs against competitors

Most of this docs site is pure operator reference — it doesn't need to "win" anyone. Use this skill only for the small number of pages whose job is to help a **prospective or new reseller evaluate or ramp up on the platform**: the quick start, and any page that doubles as reseller-program sales material.

The goal is never to copy their copy. It's to find (a) onboarding patterns that reduce a new reseller's time-to-first-tenant, and (b) things WaMatrix.io's reseller program can truthfully claim that competitors can't.

---

## Who to check

White-label / reseller programs for WhatsApp Business platforms, not the end-customer tools themselves:

| Competitor program | Docs | Watch for |
|---|---|---|
| **Wati** (Partner/Reseller program) | `support.wati.io`, partner docs | How they explain revenue share / markup model, and what setup they require before a partner can onboard their first client. |
| **AiSensy** (Partner program) | `wiki.aisensy.com` | Whether pricing/margin structure is documented publicly or gated behind a sales call. |
| **respond.io** (Partner/Agency program) | `respond.io/partners`, help docs | How multi-client/workspace management is explained — this maps directly to our tenant/sub-tenant model. |
| **Interakt** (Partner program) | `interakt.shop` | SEO-driven "become a WhatsApp reseller" content — a positioning angle, not necessarily a docs pattern. |
| **360dialog** (BSP/reseller layer) | `docs.360dialog.com` | They operate closer to true BSP/Tech-Provider layer — good source for how tech-provider/WABA approval steps get explained, directly relevant to `pa/tech-provider-approval.mdx`. |

## Method

1. **Search, don't guess.** `WebSearch` for `<competitor> white label WhatsApp reseller program setup docs`, then `WebFetch` the actual page. Structure is the data; secondhand summaries aren't.
2. **Record the setup order, not the prose.** What do they make a new reseller configure first — branding, currency/pricing, or channel/Meta connection? Compare against our own quick start order (brand → currency → plans → billing → onboard first tenant).
3. **Find the drop-off guards.** Anything framed as "before you start" or "you'll need" — these exist because their support team got tired of the same setup mistake. Free research for our own "Before you start" sections.
4. **Find what they don't explain well.** Tech-provider/WABA approval and Meta's markup/pricing model are commonly glossed over — a gap we can fill with a genuinely clear `pa/tech-provider-approval.mdx`.
5. **Cross-check every claimed advantage against the product** before it goes in a page — see `reseller-docs-fact-check`. A margin/pricing advantage we don't actually offer is worse than no claim at all.

## Our honest differentiators (verify before reusing — re-check, don't assume permanent)

1. **Flat subscription billing, no per-conversation Meta markup passed through as extra margin-taking** — worth confirming current wording against `content-brief-for-team.md` in the product repo (`/home/corbital/laravel/whatsmarkio_reseller/content-brief-for-team.md`, item 7: "WhatsApp Business API Pricing 2026") before stating it as a reseller-program claim; that brief is marketing-facing, not this docs site, so re-verify the claim applies to the reseller program itself, not just the end product.
2. **Sub-tenants inherit the reseller's Meta app automatically at signup** — reduces the reseller's own onboarding friction for each new customer; verify current behaviour via `reseller-docs-fact-check` before stating it as a setup-time saver.
3. **Full branding control** — custom domain, logo, favicon, email sender identity, legal links — verify which of these need an add-on plan before claiming "full" anything (see `pa/branding.mdx`, and the `wamatrix-reseller-docs` skill §3 on plan gating).

**Rules when using these:** never name a competitor on a docs page — describe the mechanism, not the comparison. Never state a competitor's actual pricing or revenue-share terms; they change and being wrong is worse than silence. Re-verify a differentiator before reusing it in a new page — reseller-program terms and gating move.
