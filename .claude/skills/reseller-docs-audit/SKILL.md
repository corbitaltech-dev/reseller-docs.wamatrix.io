---
name: reseller-docs-audit
description: Full-site health sweep for reseller-docs.WaMatrix.io — runs the consistency, grammar, and link-integrity checks across every page, deep-reads a representative sample per nav group, and produces a severity-ranked report with a suggested order of work. Activate for a pre-launch review, a scheduled full sweep (reseller-docs-manager runs this periodically or after any reorg), or when the user asks for a health check / audit of the reseller docs site.
---

# Full-site reseller documentation audit

The site-wide health check. Combines the mechanical sweeps (`reseller-docs-consistency-lint`, `reseller-docs-grammar-style`, `reseller-docs-link-integrity`) with a deep, line-by-line read of a representative sample, then reports everything in one severity-ranked document. This is not a new rulebook — every check here defers to `wamatrix-reseller-docs` for the rule and to the three checking skills for how to find violations; this skill owns the **process and report shape**.

---

## 1. How to run it

1. **Read `wamatrix-reseller-docs` first**, every time — rules change.
2. **Deep-read a representative sample**: `pa/quick-start.mdx` (the flagship page — it's the cross-cutting first-run flow), one page from each nav group (Getting started / Sales / Customers / Settings / Team & audit / AI Flow), and `api/overview.mdx`. Read these in full, line by line — a grep sweep alone misses grammar errors and reseller/tenant confusion.
3. **Run every mechanical check site-wide**, not sampled:
   - `reseller-docs-consistency-lint` (brand, reseller/tenant terminology, gating language, images, icons, casing)
   - `reseller-docs-link-integrity` (internal links, reorg rot, image paths, `docs.json` + `api-collection/openapi.json` validity, Meta soft-404s)
   - `reseller-docs-grammar-style` on at least the deep-read sample; note which pages weren't read line-by-line.
4. **Note what's already good.** Name specific pages/patterns worth keeping — an audit that only lists problems teaches nothing about what's working.

## 2. Severity and effort model

| Severity | Meaning |
|---|---|
| **High** | Breaks the reader's task right now, or could cause a reseller to misconfigure billing/limits for their own tenants — a 404 on click, a wrong plan-gate claim, a reseller/tenant swap in a billing page. Fix before anything else ships. |
| **Medium** | Undermines trust or consistency but doesn't cause a real misconfiguration — brand slips, casing drift, a grammar error, inconsistent terminology on a low-stakes page. |
| **Low** | Internal hygiene with no reader-facing impact — misspelled asset filenames, stale developer notes. |

| Effort | Meaning |
|---|---|
| **Trivial** | A single-line fix, no judgement. |
| **Low** | A find-replace across a handful of files, no judgement. |
| **Medium** | Requires rewriting a sentence or paragraph, but the fix is unambiguous once decided. |
| **Needs a decision** | Depends on a product/IA call only the user can make. Never resolve unilaterally — surface as an open question. |

Bucket findings by theme (lettered, A/B/C…) so the report reads as a punch list.

## 3. Report template

```markdown
# WaMatrix Reseller Documentation Audit

**Site:** reseller-docs.WaMatrix.io (Mintlify)
**Date:** <date>
**Status:** <pre-launch / live / post-reorg, etc.>
**Audited against:** .claude/skills/wamatrix-reseller-docs/SKILL.md

## 1. How this audit was done
<governance files read, pages deep-read, sweeps run>

## 2. Reproduce / verify before shipping
<every grep/mint/python command used>

## 3. Executive summary
<one paragraph, plus the severity × effort table of lettered buckets>

## 4. Findings
<one subsection per bucket; each finding: what, where (file:line), why it matters, fix>

## 5. What's already good (keep doing this)
<specific pages/patterns, named>

## 6. Suggested order of work
<numbered, cheapest-and-highest-severity first, decisions last>

## 7. Reseller/tenant terminology check
<explicit pass over wamatrix-reseller-docs §2's reseller-vs-tenant rule —
its own section because a swap here has real financial/config consequences
and is easy to miss in a general grep pass>
```

## 4. What goes in "Findings" vs. what gets escalated

- **Mechanical** (bare `WaMatrix`, bare images, icon typos, dead old-path links, `docs.json`/OpenAPI syntax errors) — state the fix directly.
- **Judgement** (reseller/tenant terminology ambiguity, unverified plan-gate claims, Title Case vs sentence case as a site convention, IA questions) — state the **options**, not a single prescribed answer, marked "needs a decision".

## 5. Common finding categories (a checklist, not exhaustive)

- Reseller/tenant terminology confusion — see `reseller-docs-consistency-lint` §2; check this even if nothing else is wrong, it's the site's highest-stakes mistake.
- Broken internal links from a reorg — see `reseller-docs-link-integrity` §2.
- Brand slips — bare "WaMatrix" outside the exceptions.
- Unverified or stale plan/add-on gating claims — route to `reseller-docs-fact-check`.
- Formatting inconsistency — bare images, icon typos, heading casing.
- Grammar and typos, especially inside tables and callouts.
- `docs.json` / `api-collection/openapi.json` drift from the real API routes.
- Information architecture — a nav group or page that no longer matches how the product is organised.

## 6. After the audit ships

The report is not the fix. Follow-up is `reseller-docs-manager`'s job: it auto-fixes what's mechanical on a scoped branch and routes the rest. Don't let an audit sit unactioned.
