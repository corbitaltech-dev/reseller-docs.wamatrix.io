---
name: reseller-docs-fact-check
description: How to verify a reseller-docs.WaMatrix.io claim against the real WaMatrix.io reseller panel product — locating the admin screen in the Laravel/Vue/Inertia source at /home/corbital/laravel/whatsmarkio_reseller, confirming exact UI button and status labels, checking plan/feature-limit gating, and routing to the right feature specialist agent already defined in that repo's own `.claude/`. Activate when writing any factual claim about how the reseller panel behaves, when a `pa/` or `api/` page mentions a button/screen/limit, when auditing an existing page for accuracy, or when the user asks whether a documented behaviour is real.
---

# Fact-checking a reseller-docs claim

Every behavioural claim on `reseller-docs.WaMatrix.io` must be traceable to the actual reseller panel. A page describing a setting, button, or gate that doesn't exist misconfigures a reseller's real billing and tenant setup — this is an operator manual, so a wrong fact here has direct financial consequences, not just an embarrassing typo.

**Product source:** `/home/corbital/laravel/whatsmarkio_reseller` — Laravel + Vue/Inertia. The reseller-facing admin screens live under `resources/js/pages/admin/**`; the reseller's own end customers ("tenants"/"sub-tenants") have their surface under `resources/js/pages/tenant/**` and `resources/js/pages/sub-tenant/**` — don't confuse the two when checking a claim. `resources/js/pages/platform/**` is the platform-owner (super-admin-of-resellers) layer, one level above what this docs site covers unless the page is explicitly about platform-level setup.

This repo already ships its own deep `.claude/` layer (agents + skills) for the product itself — reuse it rather than re-deriving conventions from scratch.

---

## Check the repo's own memory and skills first

Before grepping cold, check whether this exact area is already documented for engineering purposes:

```bash
grep -rl "<feature keyword>" /home/corbital/laravel/whatsmarkio_reseller/.claude/memory/
```

Relevant memory files already known to exist:

| Area | Memory file |
|---|---|
| Reseller currency setup | `project_reseller_currency.md` |
| Reseller plan builder / wizard | `project_reseller_plan_wizard.md` |
| Seeder library (starter content) | `project_reseller_seeder_library.md` |
| Feature-limit alerts | `project_reseller_feature_limit_alerts.md` |
| Admin API | `project_admin_api.md` |
| Sub-tenant role collisions, gotchas | `gotcha_reseller_subtenant_role_collision.md`, `gotcha_reseller_plan_feature_enabled_check.md`, `gotcha_subtenant_cannot_write_reseller_settings.md` |
| B2B platform master requirements/plan | `project_b2b_platform_docs.md` (points at `docs/B2B-MASTER-REQUIREMENTS.md` and `B2B-MASTER-PLAN.md` in that repo) |

A gotcha file is a warning that the obvious reading of the UI is wrong — read the matching one before writing a page about that area.

## Map a `pa/` doc page to its source screen

| Doc page | Admin screen(s) under `resources/js/pages/admin/` |
|---|---|
| `pa/quick-start.mdx` | (cross-cutting — first-run flow across several of the below) |
| `pa/branding.mdx` | `settings/system`, `settings/` (branding/email/legal) |
| `pa/currency-setup.mdx` | `currency/` |
| `pa/plans-and-pricing.mdx` | `plans/`, `plans/components/`, `features/` |
| `pa/billing-and-payouts.mdx` | `settings/payment/` |
| `pa/onboarding-a-sub-tenant.mdx` | `sub-tenants/` |
| `pa/subscriptions.mdx` | `sub-tenant-billing/subscriptions/` |
| `pa/invoices-and-payments.mdx` | `sub-tenant-billing/invoices/` |
| `pa/offline-payments.mdx` | `sub-tenant-billing/offline-payments/` |
| `pa/coupons.mdx` | `coupons/` |
| `pa/taxes.mdx` | `taxes/` |
| `pa/managing-customers.mdx` | `sub-tenants/` |
| `pa/plan-limits-and-customer-access.mdx` | `feature-usage/`, `features/` |
| `pa/core-platform.mdx` | `settings/system/`, `settings/` |
| `pa/infrastructure-and-security.mdx` | `circuit-breakers/`, `system-logs/`, `failed-jobs/` |
| `pa/channels.mdx` | `settings/messaging/` |
| `pa/tech-provider-approval.mdx` | `settings/integrations/` (Meta app credentials) |
| `pa/messenger-permissions.mdx` | `settings/integrations/` |
| `pa/users-and-roles.mdx` | `invitations/`, role/permission screens |
| `pa/media-library.mdx` | `media/`, `media-library/` |
| `pa/seeder-library.mdx` | `seeder-collections/` |
| `pa/activity-log.mdx` | `activity-log/` |
| `pa/ai-node-catalog.mdx`, `pa/ai-flow-exemplars.mdx`, `pa/ai-generations.mdx` | `ai-nodes/`, `ai-exemplars/`, `ai-generated/` |

If a page doesn't map cleanly, check `resources/js/pages/admin/` directly — new screens get added; this table can go stale.

## The sidebar path — one authoritative file, not the Vue component

Every `pa/` page must open its "What you see on the screen" section by saying exactly where to click in the sidebar (`wamatrix-reseller-docs` §4). **The single source of truth for this is `config/sidebarmenu.php`**, not `resources/js/components/AppResellerAdminSidebar.vue` (which only renders whatever the config provides, with no labels of its own). It's resolved into the page as `sidebarMenu` via `app/Services/Navigation/SidebarMenuResolver.php`.

Confirmed structure (re-verify before trusting if it's been a while):

- **`main` array** — the collapsible sidebar groups shown directly: `Dashboard` (flat) · `Tenant → Tenant List` · `Plan → Plan List` · `Sales → Subscriptions / Invoices / Payments / Offline Payments` · `AI Flow → Node Catalog / Flow Exemplars / Generations` · `Settings → System Settings / Payment Settings` · `Media Library → My Media / Tenant Media` (owner-only).
- **`setup` array** — flat items with **no parent group**, shown in a separate "Setup" slide-out panel opened from a button at the foot of the sidebar: `User`, `Role`, `Currency`, `Coupons`, `Taxes`, `Email Templates`, `Seeder Library`, `Activity Log`.

Write the path to match which array the item is actually in — **"Sales → Subscriptions"** for a `main`-array child, but **"Setup → Currency"** (no invented parent) for a `setup`-array item. Getting this backwards (inventing a group for a flat Setup item, or flattening a real nested one) is a factual error, not a style choice.

**Known gap:** `pa/channels.mdx`, `pa/tech-provider-approval.mdx`, and `pa/messenger-permissions.mdx` reference sidebar items with no match anywhere in `config/sidebarmenu.php` as of the last check. Don't assume these pages' sidebar claims (if any) are correct — verify them explicitly before repeating a path from an existing page, since existing pages are not evidence (see "Known traps" below). If a screen genuinely has no sidebar entry, it may be nested inside `System Settings` or reached another way — confirm before writing any path, and flag rather than guess if it can't be found.

Also note the reseller's *own* whatsmark.io subscription/invoices are deliberately **absent from this sidebar** — those live in the user menu and are read-only (a reseller buys/renews on whatsmark.io itself, not from inside their own panel). Never confuse this with the `Sales → Subscriptions` item, which is the reseller's **customers'** (tenants') billing data.

## What must be verified before it ships

- **Every UI label** — button text, menu item, page name, tab, field label, including field **placeholder/max-value text** like "Max 10000" (`plans-and-pricing.mdx` leans on these).
- **Every status/tier value** — `Active`, `PENDING`, `APPROVED`, `Lifetime`, `Unlimited`. Case matters.
- **Every navigation path** — the exact sidebar path a reseller actually clicks through, verified against `config/sidebarmenu.php` (see above), stated as the very first sentence of "What you see on the screen."
- **Every plan/feature gate** — is this screen or option really available, or gated behind an add-on / higher reseller tier?
- **Every "you cannot exceed your own plan" mechanic** — these are load-bearing claims (`plans-and-pricing.mdx`'s whole "before you start" section); verify against `FeatureService`/`features/` rather than assuming the pattern holds identically everywhere.

## How to check

**UI labels** — grep the Vue pages for the visible string:

```bash
cd /home/corbital/laravel/whatsmarkio_reseller
grep -rn "Add Plan" resources/js/pages/admin --include="*.vue" | head
```

If the string only appears via a translation/locale file, check the current value there.

**Plan gating and limits** — traced through `FeatureService` and the plan/feature seeders, same mechanism as the tenant-facing product. Grep for the feature key. If the docs and the code disagree, the code is right and the docs page needs fixing — say so.

**Behaviour** — trace the controller (`app/Http/Controllers/Admin/`) or job rather than inferring from the Vue page alone; the Vue page shows the happy path, not every edge case (e.g. what happens when a tenant already has an active subscription — see the currency page's "ISO Code cannot be changed once a tenant has an active subscription" claim, which comes from a real guard, not a UI hint).

**API tab claims** — cross-check `api-collection/openapi.json` in this docs repo against the real routes:

```bash
cd /home/corbital/laravel/whatsmarkio_reseller
php artisan route:list --path=api
```

## Route to a specialist instead of guessing

This product repo ships its own feature-specialist agents. When a claim is non-trivial, use the matching one rather than reading half a subsystem cold:

| Claim is about | Agent (in `whatsmarkio_reseller/.claude/agents/`) |
|---|---|
| Admin CRUD screens generally, resource patterns | `admin-crud` |
| Admin-side REST API | `admin-api` |
| Payment gateway setup/config | `payment-gateway` |
| Tenant billing, invoices, subscriptions, quotas | `tenant-billing` |
| Tenant-facing REST API | `tenant-api` |
| WhatsApp channel connection, embedded signup, coexistence | `whatsapp-channels` |
| WhatsApp templates, sync, approval | `whatsapp-templates` |
| Chatbot/WhatsApp Flow templates | `whatsapp-flow-templates` |
| AI chat assist / AI credentials | `ai-chat-assist`, `ai-credentials` |
| Shopify integration | `shopify` |
| Public developer API docs mechanics | `api-docs` |
| Upstream B2C → this repo sync status ("is this feature even ported yet?") | `whatsmarkio-sync` |

Only spawn one when the claim actually warrants it — a single label check is a `grep`, not an agent.

## When you can't verify

State it rather than guessing. Add a `<Note>` caveat, or leave the claim out and flag it to the user. Never soften an unverified claim into vague language to make it feel safe — vague and wrong is still wrong, and on a billing/limits page it's expensive.

## Known traps

- **This repo has diverged from the upstream `whatsmark.io` B2C product** — never assume a whatsmark.io customer-facing behaviour applies here unmodified; check `whatsmarkio-sync`'s memory (`project_whatsmarkio_sync.md`) for what's been intentionally ported vs. excluded vs. adapted.
- **"Reseller" vs. "platform" vs. "tenant" are three different layers** in this codebase (`resources/js/pages/platform`, `admin`, `tenant`, `sub-tenant`). Confirm which layer a claim belongs to before writing it — this docs site (`pa/`) covers the **reseller/admin** layer.
- **Feature gating logic has its own known gotcha**: `gotcha_reseller_plan_feature_enabled_check.md` — read it before asserting how a feature-enabled check behaves.
- **The docs can be stale.** Existing `pa/` pages are not evidence for a new page — verify against source, not against a neighbouring page.
