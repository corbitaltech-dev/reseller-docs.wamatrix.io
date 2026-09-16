---
name: reseller-docs-link-integrity
description: Finds and fixes broken links on reseller-docs.WaMatrix.io — stale internal paths after a section reorg, dead image references, docs.json validity (including the API tab's openapi.json reference), and Meta/WhatsApp developer links that return a soft-404 (HTTP 200 with an empty shell) which curl and mint broken-links cannot detect. Activate after any folder rename or navigation reorg, before launch, on a scheduled reseller-docs-manager sweep, or when a reader reports a dead link.
---

# Link integrity: internal, image, API-spec, and Meta links

Four distinct failure modes. `mint broken-links` catches the first two; it cannot catch the third or fourth.

---

## §1 — Internal links (authoritative check)

```bash
mint broken-links
```

Ground truth for internal `href`s and anchors. Run before and after any change so you can separate your own breakage from what was already there.

## §2 — Stale paths after a section reorg

A folder move or nav reorg leaves the old path referenced in cards, prose links, and accordions across the site. Treat one hit as a signal to sweep the whole repo, not the full list.

```bash
# after any rename old/path -> new/path, sweep for the old path everywhere
grep -rn --include="*.mdx" "pa/old-page-name" .
```

Cross-check every hit against the live navigation in `docs.json` — a link is only "stale" if the path it targets isn't actually a page anymore.

## §3 — Image references

This site's real screenshot convention is `<Frame>`/`<img src="/images/pa/...">`, not a bare markdown image (that's the sibling docs.WaMatrix.io site's convention). Check both forms — a bare image is itself a drift finding (see `reseller-docs-consistency-lint` §5), but its `src` still needs verifying like any other:

```bash
grep -rn --include="*.mdx" -E '<img src="|!\[' .
```

For each `src`, confirm the file exists on disk at that path (images live under `images/pa/` at the repo root, not a page-local `img/` folder):

```bash
# example: verify every referenced pa/ image actually exists
grep -rohE '/images/pa/[a-zA-Z0-9._-]+\.(png|jpg|jpeg)' pa/*.mdx | sort -u | while read -r p; do
  [ -f ".${p}" ] || echo "MISSING: $p"
done
```

Check visually in `mint dev` when in doubt.

## §4 — Duplicate assets

```bash
find images/pa -type f \( -name "*.png" -o -name "*.jpg" \) -exec basename {} \; | sort | uniq -d
```

A duplicate filename is worth a look — it can mean duplicated content, not just a duplicate file.

## §5 — `docs.json` validity, including the API tab

```bash
python3 -c "import json; json.load(open('docs.json')); print('valid')"
python3 -c "import json; json.load(open('api-collection/openapi.json')); print('valid')"
```

`docs.json`'s `"API"` tab points at `api-collection/openapi.json` — validate both files after touching either. Also confirm the OpenAPI file's referenced paths (e.g. `GET /api/v1/plans`) still match real routes if you're not sure the spec is current:

```bash
diff <(python3 -c "import json; d=json.load(open('api-collection/openapi.json')); print('\n'.join(sorted(d.get('paths',{}).keys())))") <(cd /home/corbital/laravel/whatsmarkio_reseller && php artisan route:list --path=api/v1 --json 2>/dev/null | python3 -c "import json,sys; print('\n'.join(sorted(r['uri'] for r in json.load(sys.stdin))))")
```

Run this after *any* edit to `docs.json`, including ones made by another skill — it's the file most likely to be mid-edit by another writer.

## §6 — Meta/WhatsApp developer links: the soft-404 trap

`developers.facebook.com` returns HTTP 200 with an empty navigation shell for URLs that don't exist. `curl -I` and `mint broken-links` both see a 200 and report success — neither can catch this.

**Never guess a Meta URL from its topic, and never trust a curl 200 for one.** Before any Meta link ships or is re-verified:

```
WebFetch <url> "Does this page have real documentation content, or is it an empty
placeholder / page not found? Summarise what it actually covers."
```

This matters especially for `pa/tech-provider-approval.mdx` and `pa/messenger-permissions.mdx`, the pages most likely to cite Meta's own rules.

If a Meta page can't be confirmed to exist, state the rule without a link rather than shipping a URL that might be dead.

---

## Reporting

Group findings by section (§1–§6). For §2 (reorg rot), report the count of files affected, not just the first hit. For §6, report each Meta link's verification status explicitly: confirmed-live, confirmed-dead, or not-yet-checked — never assume "not yet checked" means "fine."
