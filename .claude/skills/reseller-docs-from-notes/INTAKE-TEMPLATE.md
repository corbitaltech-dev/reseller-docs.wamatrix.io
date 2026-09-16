# Feature/screen intake — fill one per page

Keep it short. Bullet points are fine. The `reseller-docs-from-notes` skill expands this into a full page — you don't need to write prose, just give the facts and the screenshots. Anything you leave blank, the agent will either skip or ask about.

> Tip: type the **exact button, field, and menu labels** the way the panel shows them, in bold. Include real example values and field maximums where they exist ("Max 10000") — that's the one thing the agent can't guess.

---

## 1. Screen / feature name
_(exactly as it appears in the reseller panel)_
>

## 2. What it controls — one line
_(the outcome/setting, not the mechanism. "Create discount codes tenants can apply at checkout", not "coupon CRUD module")_
>

## 3. Where it lives in the panel
_(nav path, e.g. Sales → Coupons)_
>

## 4. Plan / add-on gate
_(tick one — if unsure, write "not sure" and the agent will verify)_
- [ ] Available on every reseller plan
- [ ] Needs a specific reseller plan tier
- [ ] Needs an add-on
- [ ] Not sure

## 5. Fields / settings on this screen
_(one row per field; include example value and max/min if shown in the UI)_
| Field label | Example value | Max/min or notes |
| --- | --- | --- |
| | | |

## 6. Prerequisites
_(only real ones — "a base currency must be set first", "at least one plan must exist". Leave blank if none.)_
-

## 7. Steps, in order
_(one bullet per step; put the exact button label in **bold**. Skip if this is a pure reference page.)_
1.
2.
3.

## 8. Screenshots
_(list each image file, one line on what it shows, and which step/field group it belongs to)_
| File | Shows… | Belongs to step/field group |
| --- | --- | --- |

## 9. Gotchas / common mistakes / "why can't I…"
_(these become inline Warnings/Notes and any Common questions section)_
-

## 10. Is a Meta/WhatsApp rule involved?
_(WABA approval, tech-provider status, messaging limits — the agent will cite Meta's page for it)_
>

## 11. Related pages to link
_(for "Where to next" — forward-looking pages the reader hasn't seen yet)_
-

## 12. Anything you're unsure about
_(the agent will flag these to verify against the product rather than guess)_
-

---

### Reminders the agent will enforce for you (so you don't have to)
- Brand is **WaMatrix.io** everywhere except the site's own "WaMatrix Reseller" name and `keywords:`.
- **Reseller** and **tenant/sub-tenant** must never be swapped — get this wrong and the page misdirects a real configuration decision.
- Real numbers, field maximums, and example values **belong on this site** — unlike a customer-facing product, don't scrub them out.
- Every plan/add-on-gated feature gets marked at first mention, verified against the product.
- Screenshots become plain markdown images with descriptive alt text, including the example values shown.
