---
name: reseller-docs-consistency-lint
description: Site-wide sweep for brand, terminology, formatting, and gating-language drift on reseller-docs.WaMatrix.io — bare "WaMatrix" outside keywords/site name, reseller vs. tenant/sub-tenant swaps, unmarked plan/add-on-gated features, thin or missing image alt text, Lucide icon typos, and Title Case vs sentence case drift. Activate for a pre-launch sweep, a scheduled reseller-docs-manager run, or "lint this page/section for brand and formatting issues".
---

# Consistency lint: brand, terminology, formatting

A mechanical pass over the rules in `wamatrix-reseller-docs` §1, §2, §3, §5. Grep the whole site, don't sample — this skill only *finds* violations; the rulebook has the reasoning.

**Important: this site's banned/allowed word list is the inverse of docs.WaMatrix.io's.** Don't reuse that site's lint patterns unmodified — "tenant", "quota", "webhook", "WABA" are all correct here. What's actually banned on *this* site is reseller/tenant confusion and bare "WaMatrix".

---

## §0 — Missing sidebar path

Every `pa/*.mdx` page must open its "What you see on the screen" section with the real sidebar path (`wamatrix-reseller-docs` §4, `reseller-docs-fact-check`'s sidebar section). This was found missing across **every** existing `pa/` page before the rule was added — treat it as a real, currently-outstanding finding, not a hypothetical one:

```bash
# pages with a "What you see on the screen" heading that's missing a sidebar click-path in the next line or two
grep -rLE "sidebar (menu|panel)|Setup panel" pa/*.mdx
```

For each hit, this is a **fact-check + fix** item, not pure mechanical — the real path must come from `config/sidebarmenu.php` (see `reseller-docs-fact-check`), so route to that skill before writing the path in rather than guessing from the page's own nav-group placement in `docs.json` (the docs site's grouping and the app's own sidebar grouping don't always match one-to-one).

## §1 — Bare "WaMatrix" instead of "WaMatrix.io"

```bash
grep -rnE --include="*.mdx" "WaMatrix[^.]" . | grep -v "keywords" | grep -v "WaMatrix Reseller"
```

Every hit outside `keywords:` and the site's own "WaMatrix Reseller" name should read **WaMatrix.io**. Watch for the regex matching a sentence end (`WaMatrix.` with a period — correct) — read the actual match.

## §2 — Reseller vs. tenant/sub-tenant confusion

This is the highest-value check on this site — a swapped term here misdescribes who does what to whom in a billing or access-control page.

There's no single grep for a semantic swap; instead, spot-check any sentence where both words appear close together, and read it for sense:

```bash
grep -rn --include="*.mdx" -E "\b(tenant|sub-tenant)\b.*\b(you|your)\b|\b(you|your)\b.*\b(tenant|sub-tenant)\b" .
```

Read each hit: does "you"/"your" correctly refer to the **reseller** (the reader) and "tenant"/"sub-tenant" correctly refer to **the reseller's own customer**? A sentence like "your tenant's plan limit" is fine; "you cannot exceed the tenant's plan" when the intent was the reseller's own plan is a real bug. This is a judgement check, not a mechanical one — flag, don't auto-fix.

Also confirm **tenant** vs. **sub-tenant** is used consistently with the term the *specific page* already uses — some pages say "tenant", others "sub-tenant" for the same concept; a page mixing both without reason reads as sloppy (not necessarily wrong, but worth flagging for the page's own internal consistency).

## §3 — Unmarked plan/add-on-gated features

```bash
grep -rniE --include="*.mdx" "add-on|addon|higher.?tier|paid plan|reseller plan that includes" .
```

Cross-check each hit: is the gate marked clearly at first mention (`wamatrix-reseller-docs` §3), and is it actually still accurate? A judgement + fact-check item — route to `reseller-docs-fact-check`, don't just check the wording is present.

## §4 — Lucide icon names

Font Awesome names render as nothing, silently.

```bash
grep -rn --include="*.mdx" -E 'icon="(bolt|comments|bullhorn|robot|cart-shopping|wand-magic-sparkles|mobile|file-lines|address-book|paper-plane|scale-balanced)"' .
grep -n '"icon"' docs.json | grep -E 'bolt|comments|bullhorn|robot|cart-shopping'
```

Full known-bad → correct table is in `wamatrix-reseller-docs` §5. After any fix, load the page in `mint dev` and look — a correct-looking name can still be wrong if it's not real Lucide.

## §5 — Screenshot convention and alt/caption text

**This site's real convention is `<Frame caption="...">` wrapping an `<img alt="...">`** — confirmed across 20+ existing `pa/` pages (e.g. `currency-setup.mdx`, `plans-and-pricing.mdx`). This is different from the sibling docs.WaMatrix.io site, which uses a bare `![]()` markdown image — don't apply that site's screenshot rules here.

```bash
# find any bare markdown image — on this site that's a drift from the house convention, not the standard
grep -rn --include="*.mdx" -E '!\[.{0,60}\]\(' .
# find Frame/img blocks with thin caption or alt
grep -rn --include="*.mdx" -B1 'caption=".\{0,25\}"' .
```

Flag: a bare `![]()` image (should be converted to `<Frame>`/`<img>`), or a `caption`/`alt` that's empty or just the feature name rather than a real description of what's visible (field names, example values, toggle states). `caption` and `alt` should carry the same descriptive text — flag a page where they've drifted apart. Also confirm every referenced `/images/pa/*.png` path actually exists on disk (`ls images/pa/`) — a typo'd filename fails as silently as a missing alt.

## §6 — UI-label casing consistency

The same button/field referred to with two different capitalisations across pages, e.g. "Add plan" vs "**Add Plan**".

```bash
grep -rn --include="*.mdx" -i "add plan" .
```

No single grep finds all instances site-wide — it surfaces during a close read (`reseller-docs-reviewer`) or a `reseller-docs-fact-check` pass. When found, the real UI label wins; make every reference match exactly.

## §7 — Title Case vs sentence case

```bash
grep -rnE --include="*.mdx" '^title: "([A-Z][a-z]*\s+){2,}' .
grep -rnE --include="*.mdx" '^#{2,3} ([A-Z][a-z]*\s+){2,}[A-Z]' .
```

This heuristic over-matches (proper nouns like "WhatsApp Templates") — read every hit. Site-wide convention decision (which style already dominates `pa/`?) — escalate to the user once, then apply consistently; don't decide unilaterally per page.

---

## Reporting

Report as a flat list grouped by section number, each line `file:line — current text — proposed fix`. Separate **mechanical** hits (§1, §4, §5) from **judgement** hits (§2, §3, §6, §7) — this split is what `reseller-docs-manager` uses to decide auto-fix vs escalate. State "zero hits" explicitly for sections that returned nothing, so the reader knows it was checked, not skipped.
