---
name: reseller-docs-from-notes
description: Turns a short, point-by-point admin-screen description plus screenshots into a complete, on-brand reseller-docs.WaMatrix.io page. Reads the images to see the real reseller-panel UI (labels, buttons, field maximums, states), reads the author's terse bullets, then expands them into a full MDX page following the house operator-manual structure — hook, field/setting reference, before you start, steps, warnings inline, common questions, where next — with each screenshot placed as a plain markdown image with descriptive alt text, plan-gated features marked, Meta rules cited, and every fact flagged for verification. Expansion means wording, flow, and structure only: it never invents a label, limit, or behaviour that isn't in the notes, the image, or the verified product. Activate when the user provides admin-screen notes and/or screenshots and wants them written up, says "draft a reseller docs page from these bullets", or hands over a filled intake template.
---

# Reseller docs from notes + screenshots

The author gives you a little; you produce a complete operator-manual page. Input: short bullets and one or more screenshots of a reseller-panel screen. Output: a finished MDX page that reads like the rest of `pa/`. **Expand wording, flow, and structure — never invent facts.** A label, limit, or behaviour that isn't in the notes, visible in the image, or confirmed in the product does not go on the page.

**Read `wamatrix-reseller-docs` first** — it sets the voice (operator-precise, not persuasive), brand/naming rules, Mintlify mechanics, Meta-citation rules, and the page structure you build to. This skill is the intake-to-draft method on top of it. Verify facts with `reseller-docs-fact-check`; check the finished page with `reseller-docs-reviewer`.

Note the audience difference from a typical SaaS docs site: the reader is the **reseller/admin configuring the panel**, not an end customer being sold to. Numbers, field maximums, and real example values belong on the page — they're the content, not a liability (see `wamatrix-reseller-docs` §2).

---

## What you receive

Ideally the author fills `INTAKE-TEMPLATE.md` (next to this skill). If they send loose bullets instead, ask them to map to those fields, or infer the mapping and confirm. Either way you need, per screen/feature:

- the screen/feature name **as the panel shows it**,
- one line on what setting or capability it controls,
- the fields/settings it contains, with real example values and max/min where relevant,
- the steps in order (with exact button labels), if it's a procedure rather than pure reference,
- the screenshots, each with a one-line "this shows…" and which step or field group it belongs to,
- any plan/add-on gate, prerequisite, or gotcha.

## Step 1 — Read the screenshots

For each image, record: which screen it is, the exact visible labels/button text, field placeholder or max-value text (e.g. "Max 10000"), and any status values. This is where real UI labels and limits come from.

⚠️ **Vision can misread text**, especially numbers. A value you read off an image is a *candidate*, not a fact — confirm every label, button name, and numeric max against the product with `reseller-docs-fact-check` before it ships. If the notes and the image disagree, the product source decides.

## Step 2 — Verify the facts

Before drafting, resolve anything the page will assert:

- **Labels, nav paths, field maximums** — grep the product source (`reseller-docs-fact-check`).
- **Plan/add-on gate** — is this screen or option available on every reseller plan, or gated? Never guess; a wrong gate misleads a reseller's own commercial planning.
- **Cross-cutting mechanics** — e.g. "you cannot offer more than your own plan allows" style constraints — verify these are still accurate for the specific feature, don't assume the pattern is universal.
- **Meta rules** — if a constraint (WABA approval, tech-provider status, messaging limits) is Meta's, cite Meta's page for it.

Anything you can't verify: draft around it and **flag it to the author**, or leave a `<Note>` caveat.

## Step 3 — Choose the structure

Map the notes to the house pattern (`wamatrix-reseller-docs` §4). Most `pa/` pages are **reference-heavy**, not funnel-shaped — use only what fits:

| Section | Build it from | Skip when |
| --- | --- | --- |
| **Hook** (1–2 sentences) | the one-line "what it controls" | never — always lead with what the screen does |
| **Field/setting table** | the fields list with example values | the page is a pure procedure with no config screen |
| **Before you start** (`<Warning>`/`<Note>`) | real prerequisites, esp. anything that disables a button until met | there are no real prerequisites |
| **Steps** (`<Steps>`) | the ordered step bullets | the page is pure reference (a settings glossary) |
| **Inline warnings** | any gotcha with financial/operational consequence | none exist |
| **Common questions** | genuine ambiguity or "why can't I…" | nothing non-obvious |
| **Where to next** | related pages, forward-looking | nothing natural to point to |

Momentum rule still applies (`wamatrix-reseller-docs` §4): point forward, not back to a page already covered.

## Step 4 — Write the prose

Expand each bullet into short, simple sentences — small everyday words, one idea per sentence, no stiff or "official" vocabulary. Many readers of this site read English as a second language, so plain beats precise-but-fancy every time a plain word says the same thing. Explain any technical term (WABA, webhook, tenant, quota) in one short clause the first time it appears on the page, even though the audience is admins — don't assume it's already known.

State the mechanism, not reassurance — this reader wants to know exactly what a setting does and what breaks if they get it wrong, said simply. Use exact UI labels in **bold**. Mark every plan/add-on-gated feature right where you mention it. Do not add persuasive padding ("don't worry", "it's easy") — this isn't a sales page. See `wamatrix-reseller-docs` §1 for the full voice rule and word-choice table, and §4 for the exact page flow (hook → what you see on the screen → before you start → steps → reference tables → lifecycle → connections → questions → next) to build the draft against.

## Step 5 — Place the screenshots

- Save each image under `images/pa/` at the repo root (check that folder for the existing naming pattern), with a descriptive name (`plans-add-plan-form.png`, not `image3.png`).
- Wrap every screenshot in a `<Frame>` with a matching `caption`, the real convention on this site (not a bare markdown image — that's the sibling docs.WaMatrix.io site's convention, not this one):
  ```mdx
  <Frame caption="Describe exactly what is on screen, including example values shown">
    <img src="/images/pa/plans-add-plan-form.png" alt="Describe exactly what is on screen, including example values shown" />
  </Frame>
  ```
- Write the same real description for both `caption` and `alt` — fields, their example values, toggle states — so a reader who can't see the image still knows what the screen contains.
- Never reference an image file that doesn't exist on disk yet — confirm with `ls images/pa/` first, or ask the author for the screenshot.
- Interleave images beside the step or field group they illustrate rather than front-loading them. Blank line before and after each one.

## Step 6 — Frontmatter

```yaml
---
title: "Sentence case title"
description: "One sentence describing what this page covers."
icon: "lucide-name"
---
```

## Step 7 — Gaps: ask, don't invent

Ask a focused question only when a genuine gap blocks the page — an unclear plan gate, a field's real maximum, a step's outcome you can't tell from notes or image. Otherwise draft, and state any assumption inline so the author can correct it. Deliver everything not blocked first; batch remaining questions.

## Step 8 — Review before it's "done"

- Show the author the **rendered copy**, not a diff or file path.
- Run `reseller-docs-reviewer` (lint + grammar + links) on the new page.
- `mint dev`, `mint broken-links`, confirm icons rendered (Lucide fails silently).

---

## Worked example (compact)

**Author's input:**
> Screen: Coupons. What: create a % or flat-amount discount code resellers apply to a tenant's subscription. Plan: available on every reseller plan. Steps: 1) Sales → Coupons → **Add Coupon** 2) set **Code**, **Discount Type** (Percentage/Fixed), **Value**, optional **Expiry Date** and **Max Redemptions** 3) **Save**. Screenshot: `coupon-form.png` = the add form (step 2). Gotcha: a coupon with no Expiry Date never expires; Max Redemptions blank means unlimited uses.

**Draft output (excerpt):**
```mdx
---
title: "Coupons"
description: "Create discount codes tenants can apply to their subscription."
icon: "ticket-percent"
---

Coupons are discount codes you create and share with tenants to apply at checkout or renewal.

## Create a coupon

<Steps>
  <Step title="Open the coupon form">
    Go to **Sales → Coupons** and click **Add Coupon**.
  </Step>
  <Step title="Set the discount">
    Enter a **Code**, choose **Discount Type** (**Percentage** or **Fixed**), and set the **Value**.

    <Frame caption="The Add Coupon form, with Code set to WELCOME10, Discount Type set to Percentage, Value set to 10, and empty Expiry Date and Max Redemptions fields">
      <img src="/images/pa/coupon-form.png" alt="The Add Coupon form, with Code set to WELCOME10, Discount Type set to Percentage, Value set to 10, and empty Expiry Date and Max Redemptions fields" />
    </Frame>

    <Note>
      Leave **Expiry Date** blank for a coupon that never expires, and **Max Redemptions** blank for unlimited uses.
    </Note>
  </Step>
  <Step title="Save">
    Click **Save**. The coupon is available to apply immediately.
  </Step>
</Steps>
```

Notice: labels are bold and exact, the screenshot sits beside its step in a `<Frame>` with a matching caption/alt including the example values shown, the "blank means unlimited/never expires" gotcha became a `<Note>` at the point it matters, and nothing beyond the author's notes was invented. Before shipping, **Add Coupon / Code / Discount Type / Percentage / Fixed / Value / Expiry Date / Max Redemptions** get confirmed via `reseller-docs-fact-check`, and the image path is confirmed to exist under `images/pa/`.
