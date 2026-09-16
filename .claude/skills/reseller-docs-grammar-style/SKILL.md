---
name: reseller-docs-grammar-style
description: Proofreading pass for reseller-docs.WaMatrix.io — grammar, missing/wrong verbs, singular-plural mismatches, subject-verb agreement, and broken-sentence checks, applied to operator-manual prose (field/setting reference tables, procedural steps, inline warnings) rather than marketing copy. Activate on any new or rewritten page before it ships, on a full-site proofreading sweep, or when the user asks to check a page for typos or grammar.
---

# Grammar and style proofreading

A line-by-line proofread, not a lint. The other skills catch mechanical brand/link/format drift; this one catches sentences that are simply wrong — missing verbs, wrong tense, subject-verb mismatches, singular/plural slips. British spelling throughout, matching the rest of `.claude/`.

This site is denser with tables and field-by-field reference than a typical marketing-adjacent docs site — proofread table cells and callouts as carefully as prose paragraphs; a wrong word in a `<Warning>` about billing behaviour is higher-stakes than the same slip in a hook sentence.

---

## What to check, line by line

- **Every sentence has a verb**, and it's the right one — including inside table cells and `<Note>`/`<Warning>` bodies, which get proofread less often than headline prose.
- **Subject-verb agreement**, especially in sentences distinguishing the reseller ("you") from the tenant/sub-tenant (third person) — a common failure mode is a verb agreeing with the wrong subject when both appear in one sentence ("you cannot exceed what a tenant's plan allow" → *allows*, and check which noun the verb should track).
- **Singular/plural consistency**, especially around limits and counts: "up to 10,000 contacts", not "up to 10,000 contact"; "the plan's limits" vs "the plan's limit" — pick the one that matches what's actually configurable (one aggregate limit vs. several).
- **Capitalisation of ordinary verbs mid-sentence** — a UI label's capitalisation shouldn't bleed into the surrounding prose: "You **Add** a plan" should be lowercase *add*; only the bolded UI label keeps its real-app capitalisation.
- **Field-reference table consistency** — a table with a "Purpose"/"Example"/"Notes" column style should use the same grammatical form (sentence fragments vs. full sentences) down the whole column; a mismatch reads as sloppy even if each cell is individually correct.
- **Dangling or garbled clauses at the end of a sentence** — often where a page was edited and a clause got half-removed. Read the whole sentence; if it doesn't parse, it's not a style nit.
- **Plain-word check (this site's own extra pass).** Flag any stiff or "official" word that has a shorter, everyday replacement — *utilise → use*, *initiate → start*, *configure → set up* (unless it's the literal button text), *facilitate/leverage/streamline/comprehensive* → cut or replace. Flag any sentence over roughly 25 words or with more than one "which"/"that" clause — split it. Flag a technical term (WABA, webhook, quota, tenant vs. sub-tenant) used with no explanation on the page it first appears, even on this admin-audience site — see `wamatrix-reseller-docs` §1. This check exists because a large share of readers use English as a second language; a grammatically correct but stiff sentence still fails them.

## Method

1. Read the page start to finish, at prose speed — not a `grep` pass. Include every table cell and callout body, not just paragraph prose.
2. For every fix, quote the **current text**, the **fix**, and a one-line reason if it's not obvious (agreement, missing verb, singular/plural).
3. Don't rewrite voice or restructure while proofreading — that's the writer's job. Fix the sentence that's broken; leave a correct-but-plain sentence alone.
4. If a sentence is ambiguous about which of two entities (reseller vs. tenant) it refers to, flag it as a fact/clarity issue, not just grammar — see `reseller-docs-consistency-lint` §2. A confidently "fixed" sentence that still refers to the wrong party is worse than a flagged one.

## Reporting

One table per page: `location — current text — fix`. State the total error count and separate "sample read" pages (deep-read, exhaustive) from pages only grep-checked, so `reseller-docs-manager` knows which pages still need a full read.
