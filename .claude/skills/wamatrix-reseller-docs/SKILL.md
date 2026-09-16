---
name: wamatrix-reseller-docs
description: Authoritative rules for writing and editing any page in the reseller-docs.WaMatrix.io Mintlify site — simple, everyday English for a reseller/platform-admin audience that includes many second-language English readers, the exact page tone and flow copied from the sibling docs.WaMatrix.io site (hook → what you see on the screen → before you start → steps → reference tables → lifecycle → connections → common questions → what's next), the WaMatrix.io brand rules, honest plan-limit and gating language, Mintlify component and docs.json mechanics (Lucide icons, the index.mdx root rule, the API tab), screenshot and image alt-text conventions, the shared-repo safety rules for working alongside other writers, and the verification checklist every page must pass. Activate for ANY task that creates, rewrites, reviews, or fact-checks a reseller docs page, edits docs.json or navigation, or when the user mentions the reseller docs site, the `pa/` pages, Mintlify, or the API tab.
---

# WaMatrix Reseller Documentation

You write the docs at `reseller-docs.WaMatrix.io` (site name "WaMatrix Reseller"). The audience is a **reseller running their own white-label WhatsApp SaaS on top of WaMatrix.io** — an agency owner, or their ops/support staff. They are not a developer (except when reading the **API** tab), and they are not the end customer either: they are the platform admin who configures the panel and sells subscriptions to their own tenants.

Two goals, in this order:

1. **Understand** — a reseller grasps what a setting or screen controls and what happens if they get it wrong.
2. **Operate safely** — they can configure billing, plans, channels and tenants without breaking something for their own paying customers.

This is an **operator manual**, not a marketing funnel. Precision beats persuasion here: a reseller who misreads a limit or a gate can misconfigure pricing for hundreds of their own tenants. Everything below serves that. When a rule conflicts with elegance, the rule wins.

---

## 1. Voice: simple English, precise, second person

Write like you're briefing a competent admin who is about to click the button — but write in the **simplest English that says the fact correctly**. Assume the reader may not have English as a first language (a lot of resellers and their support staff read this site as a second-language reader, including in India, where WaMatrix.io has many resellers). That means:

- **Short sentences. One idea each.** If a sentence has two "and"s or a comma-separated list of clauses, split it.
- **Small, everyday words over big or "official-sounding" ones.** Say "use", not "utilise". Say "start", not "initiate". Say "show", not "display" or "render". Say "turn on/off", not "toggle" as a verb (use **Toggle** only as the literal UI control name, in bold). Say "set up", not "configure" or "provision", except when "Settings"/"configure" is the literal button text.
- **No filler or "corporate" words at all** — cut *leverage, utilise, seamless, robust, powerful, synergy, streamline, empower, facilitate, comprehensive, holistic*. These don't just sound stiff, they slow down a reader translating in their head. If a plain verb says the same thing, use the plain verb.
- **Explain any word a beginner might not know**, the first time it's used on a page, in the same breath — the way `campaigns.mdx` does: *"You need an approved template first."* then immediately explains what a template is where needed. Don't assume the reader already knows platform jargon (WABA, webhook, tech provider, embedded signup) just because they're an admin — define it in one short clause on first use, then use it freely after that.
- **Short, direct sentences instead of long explanations.** Prefer three short sentences to one long one joined with semicolons or "which" clauses.
- **Second person throughout** ("you" = the reseller, reading this page).

**This site's vocabulary is different from the customer-facing docs.WaMatrix.io site — do not import its banned-word list.** On *this* site the reader is the platform operator, so these real product/architecture terms are correct and expected (they are not "big words" to avoid — they are the actual name of the thing):

| Use freely here (reseller/admin audience) | Why it's fine on this site |
|---|---|
| tenant / sub-tenant | This is the actual UI and DB term for the reseller's own customer. Banned only on the customer-facing site, where "workspace" replaces it. |
| plan, feature limit, quota | The reseller is literally setting these up. |
| webhook, API, endpoint, token, scope | Real content on the **API** tab, and reseller admins do connect webhooks and payment gateways. Explain each term in one short clause the first time it's used on a page. |
| WABA / WhatsApp Business Account, embedded signup | The reseller sets up Meta app details and approves tenant WABAs — this is their actual job (see `pa/tech-provider-approval.mdx`, `pa/messenger-permissions.mdx`). Explain what the letters stand for on first use. |

**Explain the why, not just the what — in one short, plain sentence.** "This field is off" is frustrating; "This stays off until you set a base currency, because a plan needs a currency before it can have a price" is clear and simple. Whenever a limit or gate comes from the reseller's *own* WaMatrix.io subscription (rather than being a UI bug), say so, in plain words — see `pa/plans-and-pricing.mdx`'s "You cannot offer more than your own plan allows" for the pattern to match.

**Don't pad with reassurance the reader doesn't need.** This is an admin, not a nervous first-time buyer — skip "don't worry" framing and get straight to what the setting does.

**A simple test before you ship a sentence:** could you say this out loud to a shop owner or small-agency admin over a phone call, in one breath, and have them understand it first time? If a sentence needs a second read, shorten it or swap a word.

---

## 2. Brand and naming — non-negotiable

- The product is **WaMatrix.io**. Never bare "WaMatrix" in body prose, headings, descriptions, captions, or `docs.json`'s `name`.
  - `keywords:` frontmatter is the one exception — keep bare "WaMatrix" there too, for search.
  - The site's own display name, "WaMatrix Reseller" (`docs.json` → `name`), is a proper-noun exception — don't "fix" it to "WaMatrix.io Reseller".
- **Reseller** = the person/company running the white-label platform (your reader). **Tenant** (or **sub-tenant**, matching the exact page it appears on) = the reseller's own customer. Never swap these — a sentence that confuses who is who is actively dangerous in a billing or limits page.
- The reseller's *own* WaMatrix.io subscription is a separate concept from the *plans the reseller sells to tenants* — a page that blurs these two "plan" concepts will misconfigure someone's pricing. When ambiguous, write "your WaMatrix plan" vs. "the plan you're creating for tenants".

### Numbers and limits are allowed here — unlike the customer site

This is the opposite rule from docs.WaMatrix.io. There, specific limits/prices are banned outside one page because they go stale and mislead a buyer. Here, the reader is **configuring** limits and pricing directly in their own panel, so:

- Screenshots and prose **may** show real example numbers ("Max 10000", "₹799/month") because they illustrate the mechanism, not a promise about what WaMatrix.io itself costs.
- Never state what the **reseller's own WaMatrix.io subscription** costs or what its exact limits are as a hard fact on this site — that number lives on the marketing/billing side and changes; if you need to reference it, say "your own WaMatrix plan's limit" and point at **My Subscription** in the app rather than a number.
- Do state real UI defaults, field maximums, and example values when documenting how a *reseller-configured* plan or limit works — that's the whole point of this site.

---

## 3. Be precise about plan-level gating

If a panel feature or screen is only available on certain WaMatrix.io reseller plans (e.g. a paid add-on, white-label domain, or a higher-tier reseller plan), say so at the point you mention it, the same pattern as the customer site:

```mdx
<Card title="Custom domain" icon="globe" href="/pa/branding#custom-domain">
  Serve the panel from your own domain instead of the shared one. *(Needs a reseller plan that includes this add-on.)*
</Card>
```

**Verify current gating against the product before claiming anything** — see `reseller-docs-fact-check`. Never guess whether something is gated; a wrong claim here can misdirect a reseller's own commercial decisions.

---

## 4. Page structure and tone — the exact flow to match

The sibling `docs.WaMatrix.io` site (customer-facing) has a proven page flow and a very specific, warm-but-plain tone. Use the **same flow and tone** here, adapted only in vocabulary (§1) and content depth (numbers/limits allowed, §2) — not in structure. Study `/home/corbital/products-docs/docs.whatsmark.io/core-setup/canned-replies.mdx`, `messaging/campaigns.mdx`, and `core-setup/custom-fields.mdx` as the reference exemplars; every rule below is extracted from them.

### Title and description — short, plain, benefit-first

- **`title`** — the feature name exactly as the UI shows it, sentence case, 1–3 words where possible: `"Canned Replies"`, `"Campaigns"`, `"Custom Fields"`. Not a description dressed up as a title.
- **`description`** — one plain sentence stating **what it does for the reader**, not a category label. Compare:
  - ✅ `"Save the replies you send often and drop them into a chat with a shortcut."`
  - ✅ `"Send a WhatsApp or Messenger message to many contacts at once."`
  - ❌ `"Canned reply management module."` — describes the feature as software, not as something the reseller does.
  - On this site, translate the same instinct to the operator's job: `"Create and manage the plans you sell to your tenants."` (the real `plans-and-pricing.mdx` description) is exactly this pattern — what the reseller *does*, not what the screen *is*.

### The hook — state the everyday problem before the mechanism

The opening paragraph (before any heading) never starts with "Welcome to" or a feature-name restatement. It opens with the **situation that makes this feature useful**, in one or two short sentences, then names the mechanism:

> "Some questions come up again and again. Your prices, your opening hours, how to track an order. Instead of typing the same answer every time, you save it once here and drop it into a chat by typing `/`." — `canned-replies.mdx`

> "A campaign sends the same message to many contacts at once. Use it for an offer, an announcement, or a follow-up to a group of people." — `campaigns.mdx`

> "Custom fields let you store your own details on every contact, beyond name and phone number. For example: 'Company Name', 'Contract Value', 'Preferred Language', or 'Birth Date'." — `custom-fields.mdx`

On this site, the "everyday problem" is a reseller's operational one, not an end-customer one — e.g. for a reseller feature: *"Every tenant needs a price and a limit before they can subscribe. Instead of negotiating each one by hand, you set them once here and every tenant who signs up gets the same terms."* Concrete, situational, no throat-clearing.

### "What you see on the screen" — say where to click, then describe the list/table field by field

Immediately after the hook, for any page backed by a list or table screen, add a short section (titled to match, e.g. "What you see on the screen"). **Its first sentence always says exactly where in the panel's own sidebar to click to reach this screen** — the same way `managing-contacts.mdx` opens its steps with "Click **Contacts** in the Sidebar menu." Then walk the **columns or fields visible on the main screen**, one bullet per field, each field name in **bold**:

```mdx
## What you see on the screen

Click **Contacts** in the Sidebar menu to open this page.

The page lists the replies you can use — the ones your team shares, plus any you have kept to yourself:

- **Title** — the name you gave the reply.
- **Shortcut** — what you type after `/` to use it.
- **Body** — the first part of the reply text.
```

**Never guess the sidebar path or invent a menu label.** Verify the real path via `reseller-docs-fact-check` against `config/sidebarmenu.php` in the product source (`/home/corbital/laravel/whatsmarkio_reseller`) — this is the single authoritative file for every sidebar label and its parent group, not the Vue sidebar component itself (which only renders whatever this config provides). Confirmed structure as of the last check: a `main` array (collapsible sidebar groups — `Dashboard`, `Tenant → Tenant List`, `Plan → Plan List`, `Sales → Subscriptions/Invoices/Payments/Offline Payments`, `AI Flow → ...`, `Settings → System Settings/Payment Settings`, `Media Library → My Media/Tenant Media`) and a flat `setup` array shown in a separate "Setup" slide-out panel with **no parent grouping** (`User`, `Role`, `Currency`, `Coupons`, `Taxes`, `Email Templates`, `Seeder Library`, `Activity Log`). Write the path as **Sales → Subscriptions** for a `main`-array item, or just **Setup → Currency** (no group) for a `setup`-array item — match the real structure, don't invent a parent group for a flat Setup item or vice versa.

If a `pa/` page's screen has no match anywhere in `config/sidebarmenu.php` (this has already happened — `pa/channels.mdx`, `pa/tech-provider-approval.mdx`, and `pa/messenger-permissions.mdx` reference sidebar items that don't exist in the current config), **do not invent a path** — flag it as unverified and route to `reseller-docs-fact-check` to find where the screen actually lives (it may be nested inside an existing group like System Settings) before publishing a sidebar-path claim.

This is the single highest-value opening line for an admin screen — it lets the reader physically find the screen before anything else is explained. On this site, apply it to every `pa/` page that documents a screen, exactly the same way `managing-contacts.mdx` and `canned-replies.mdx` do.

### Creating/configuring something — steps, with the form's fields explained right after

For a create/edit form, don't just say "fill in the form" — after the `<Steps>` (or directly, for a single-screen form), list **every field**, bolded, with what it means and any real constraint (character limit, uniqueness, format):

> "**Shortcut** — what agents type after `/`, like `pricing`. Letters, numbers, hyphens and underscores only. Up to 50 characters, and each shortcut can only be used once." — `canned-replies.mdx`

This is exactly the field-reference-table pattern this rulebook already asks for (see the table form in `plans-and-pricing.mdx`, `currency-setup.mdx`) — bullets are the prose equivalent when a table would be overkill for 3–5 fields; a table is better for 6+ or when every field needs the same 2–3 columns (Type / Use it for / Example, as in `custom-fields.mdx`'s field-types table).

### Multi-step wizards — one `<Step>` per screen, image right where it's shown, fields listed under it

For a multi-screen wizard (`campaigns.mdx`'s 4-step campaign builder is the model): one `<Step>` per screen, the screenshot immediately inside the step, then the fields on that screen as bullets directly below the image — not before it. A `<Warning>` goes inside the step where the risk actually occurs ("Once a campaign starts sending you cannot edit it"), not collected at the bottom.

### Status and reason tables — real, exact values

Any screen with a status/state column gets its own table, one row per value, **What it means** in plain operational terms:

```mdx
## Campaign statuses

| Status | It means |
| --- | --- |
| **Draft** | Made but not sent yet. You can still edit it, and send it when you're ready. |
| **Queued** | Scheduled and waiting for its send time. |
```

Same pattern for failure/rejection reasons a reseller will see and need to explain to themselves or a tenant (see `campaigns.mdx`'s "Common Reasons for Failed Messages"). On this site this applies to subscription statuses, invoice statuses, coupon states, tenant-onboarding statuses — anywhere the panel shows a state word.

### Full page flow, in order

1. **Hook** (1–2 sentences, situational, see above) — no heading.
2. **What you see on the screen** — field-by-field bullets, when the page is backed by a list/table view.
3. **Before you start** — genuine prerequisites as a `<Warning>` or `<Note>`, especially anything that gates a button or disables a field until met (currency before plans, a base plan before add-ons). Highest-value section — prevents a support ticket from a confused reseller.
4. **The steps** — `<Steps>` for a real procedure, each step's screenshot and field bullets together (see above); skip for pure reference pages.
5. **Reference tables** — field types, statuses, failure reasons — wherever the screen has a fixed set of values worth naming.
6. **Editing/deleting/managing** — a short section for the lifecycle actions (edit, toggle active, delete), with the destructive one flagged: *"Deleting a custom field cannot be undone... If you are not sure, turn the field off with the Active toggle instead."*
7. **Where it appears elsewhere / how it connects** — when a feature's data surfaces in other screens (custom fields in imports, in merge tokens, in Flows), a short bulleted list of each place, each one linked.
8. **Common questions**, only where real ambiguity or a genuine "why can't I…" exists — don't force an AccordionGroup onto a page that doesn't need one.
9. **What's next** — a `CardGroup cols={2}`, same momentum rule as the customer site: point to pages not yet covered by this one, never backward.

Not every page needs every section — `custom-fields.mdx` has no "Common questions" because there's no real ambiguity to answer; `campaigns.mdx` skips "What you see on the screen" as a standalone section because the list view is covered later, after the creation flow. Match sections to what the page actually needs, but keep the **order** (hook → screen/fields → prerequisites → steps → reference tables → lifecycle → connections → questions → next) wherever multiple sections are present.

**Use the real UI labels, in bold, exactly as they appear in the app**: **Add Plan**, **Create Contact**, **Sync Templates**, **Enable Live Booking**. Status/tier values exactly as shown: **Active**, **PENDING**, **APPROVED**, **Lifetime**, **Unlimited**. Getting these wrong is the fastest way to look like you've never used the panel.

### Tone checklist, extracted from the exemplars

- **Simple English first.** Short sentences, one idea each, small everyday words, second person throughout. Write for a reader who may be reading English as their second language — an Indian small-agency owner and their support staff are a real, large part of this audience, not an edge case. If in doubt, use the shorter, plainer word.
- Explain a constraint's *reason*, briefly, in the same sentence, in plain words: "It has to be different from your other replies" — not just "must be unique". Say "must be unique" only if you then immediately restate it in plain words in the same breath.
- Explain any technical word the first time it appears on a page, in a short clause, even for admin-only terms like WABA or webhook — don't assume the reader already knows it just because they're a paying operator.
- A `<Tip>` for a genuinely helpful shortcut or reassurance, a `<Note>` for a neutral fact worth flagging, a `<Warning>` only for something that loses data, costs money, or can't be undone — don't upgrade a `<Note>` to a `<Warning>` for emphasis.
- Cross-link the first time a related concept is named — `[template](/messaging/whatsapp-templates)`-style inline links — rather than a bare mention with no path to it.
- End of page: a small, forward-only `CardGroup`, each card one plain sentence on the benefit of clicking through, not a repeat of the target page's own title.
- **Before shipping a page, read it out loud in your head as if explaining it to a shop owner over a phone call.** Any sentence that needs a second read, or any word that needs a dictionary, gets simplified.

---

## 5. Mintlify mechanics

### The root page rule

**`/` always renders `index.mdx`.** A `redirects` entry with `"source": "/"` **does not fire** while `index.mdx` exists — silently ignored. To make a page open first, put its content in `index.mdx` and redirect the old path *to* the root:

```json
"redirects": [
  { "source": "/pa/quick-start", "destination": "/" }
]
```

Verify with `curl -s -o /dev/null -w "%{http_code} %{redirect_url}\n" http://localhost:3000/` — don't assume.

### Icons — Lucide, not Font Awesome

`docs.json` sets `"icons": { "library": "lucide" }` (confirm this is still set before assuming it). Font Awesome names render as **nothing**, silently.

| Wrong (Font Awesome) | Right (Lucide) |
|---|---|
| `bolt` | `zap` |
| `comments` | `messages-square` |
| `bullhorn` | `megaphone` |
| `robot` | `bot` |
| `cart-shopping` | `shopping-cart` |
| `mobile` | `smartphone` |
| `file-lines` | `file-text` |
| `address-book` | `contact` |
| `paper-plane` | `send` |
| `scale-balanced` | `scale` |

Lucide has no WhatsApp brand icon — use `message-circle`.

### Components

`<Steps>/<Step>`, `<CardGroup cols={2|3}>/<Card>`, `<AccordionGroup>/<Accordion>`, `<Note>`, `<Tip>`, `<Warning>`, `<Info>`, `<Check>`. `<Card>` takes `icon`, `href`, `horizontal`. `<Step>` and `<Accordion>` take `icon`. Tables are used heavily on this site for field/setting reference — prefer a table over prose when documenting more than two related fields (match `plans-and-pricing.mdx`, `currency-setup.mdx`).

### Screenshots — `<Frame>`, not a bare markdown image

**This is different from the sibling docs.WaMatrix.io site, which uses a bare `![]()` image.** This repo's real, already-established convention (used across all 20+ existing `pa/` pages, e.g. `currency-setup.mdx`, `plans-and-pricing.mdx`) is a `<Frame>` wrapping an `<img>`, both with the same descriptive text:

```mdx
<Frame caption="The Edit Currency panel, showing the Currency Name, ISO Code and Symbol fields">
  <img src="/images/pa/currency-edit-sheet.png" alt="The Edit Currency panel, showing the Currency Name, ISO Code and Symbol fields" />
</Frame>
```

- Write the same real, descriptive text for both `caption` and `alt` — say what's actually on screen (field names, example values, toggle state), not just the feature name.
- Images live under `/images/pa/<page-or-feature>-<view>.png` at the repo root (not a page-local `img/` folder) — check `images/pa/` for the existing naming pattern before adding a new file.
- Never invent an image path — confirm the file actually exists on disk (`ls images/pa/`) before referencing it, and never write a `<Frame>`/`<img>` block for a screenshot that hasn't actually been captured yet; ask the author for it instead.

### The API tab

`docs.json` has a second `"tab": "API"` driven by `api-collection/openapi.json`, plus `api/overview.mdx`. Any change to a documented endpoint's behaviour, request/response shape, or auth scope must be reflected in the OpenAPI spec in the **same change** — don't let the spec and the prose drift. Endpoint pages generated from the spec are not hand-edited `.mdx`; if a description needs to change, edit the spec.

### Frontmatter

```yaml
---
title: "Sentence case title"
description: "One sentence describing what this page covers."
icon: "lucide-name"
---
```

(`keywords:` is optional on this site; add it only where SEO matters, e.g. a page likely to be found via search rather than only navigated to.)

---

## 6. Meta rules must still cite Meta

Any page describing a rule that comes from **Meta/WhatsApp** rather than from WaMatrix.io — WABA approval, tech-provider approval, messenger permissions, template categories, messaging limits — must link to Meta's own page for it. `pa/tech-provider-approval.mdx` and `pa/messenger-permissions.mdx` are the pages most likely to need this.

Phrase it as a source, one link at the end of the section:

```mdx
<Note>
  This approval step is controlled by Meta, not WaMatrix.io. Meta's own documentation covers it under
  [tech provider requirements](https://developers.facebook.com/documentation/business-messaging/whatsapp/...).
</Note>
```

### ⚠️ Verify every Meta link before shipping it — a 200 is not proof

`developers.facebook.com` serves HTTP 200 with an empty shell for URLs that don't exist. `curl -I` cannot tell the difference. Before any Meta link ships, `WebFetch` it and confirm real body content:

```
WebFetch <url> "Does this page have real documentation content, or is it an empty
placeholder / page not found? Summarise what it actually covers."
```

If you can't confirm a page exists, state the rule without a link rather than shipping a dead one.

---

## 7. Working in a shared repo — read before editing

**Other writers work in this repo at the same time.** Before any editing session:

```bash
git status --porcelain          # must be clean
git fetch origin && git log --oneline HEAD..origin/dev   # what is incoming? (dev is the working branch here)
git pull
```

Then:

- **Only touch the files your task is actually about.** No repo-wide sweeps, however correct, without being asked.
- **`docs.json` is shared.** Keep your diff to the smallest possible number of lines.
- **Check `git log` on a file before rewriting it.** If someone committed to it recently, say so and confirm before replacing their work.
- **Never resolve a conflict by discarding the other side.** Stash, inspect both, then decide with the user.
- If a link in a file you must not touch would break, add a **redirect** in `docs.json` rather than editing their file.

---

## 8. Verification — every page, every time

```bash
mint dev                # preview; note the port, it moves if 3000 is taken
mint broken-links       # must not add any new failures
python3 -c "import json; json.load(open('docs.json')); print('valid')"
```

Then confirm by hand:

- [ ] "WaMatrix.io" everywhere except `keywords:` and the site's own "WaMatrix Reseller" name
- [ ] Reseller vs. tenant/sub-tenant used correctly — never swapped
- [ ] Every plan-gated or add-on feature marked at first mention
- [ ] Every icon is a real Lucide name (check the rendered page — they fail silently)
- [ ] Every UI label, status value, and field maximum matches the actual reseller panel
- [ ] Any Meta/WhatsApp rule cited links to a verified-live Meta page
- [ ] `mint broken-links` shows no *new* breakage (baseline before your change)
- [ ] `docs.json` still parses if touched
- [ ] `git status` lists only the files your task was about

**Report the baseline separately from your own breakage.**

---

## 9. Related skills

- **`reseller-docs-fact-check`** — verify a claim against the real reseller panel product source at `/home/corbital/laravel/whatsmarkio_reseller`.
- **`reseller-docs-competitor-benchmark`** — before writing a page meant to help a reseller evaluate or compare against other white-label WhatsApp platforms.
- **`reseller-docs-from-notes`** — turn short feature notes + screenshots into a finished page.
- **`reseller-docs-consistency-lint`**, **`reseller-docs-grammar-style`**, **`reseller-docs-link-integrity`**, **`reseller-docs-audit`** — monitoring and QA.
