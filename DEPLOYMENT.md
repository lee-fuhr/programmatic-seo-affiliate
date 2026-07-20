# Deployment note — Pressure Washer Pro

## Cloudflare Pages (free tier, recommended)

### One-time setup

1. Push the `site/` folder to a new GitHub repo (e.g. `pressurewasherpro-site`).
2. In Cloudflare dashboard → Pages → Create a project → Connect to Git.
3. Select the repo. Build settings:
   - **Build command:** *(leave empty — this is a pre-built static site)*
   - **Build output directory:** `/` (root of the repo, since HTML is at `site/`)
   - Or: set output directory to `site` if you push the whole project root
4. Deploy. Cloudflare assigns `your-project.pages.dev` immediately.
5. Add a custom domain: Pages → Custom Domains → add `pressurewasherpro.com`.
   - Domain register at Namecheap/Cloudflare Registrar (~$10/yr for .com)

### Cloudflare Pages config file (optional — put at site root)

Create `_headers` file in the `site/` folder:

```
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
```

### Redirects (optional)

Create `_redirects` file:

```
/sitemap  /sitemap.xml  301
```

## Amazon Associates signup

1. **Email alias to use:** `lee+pressurewasherpro@leefuhr.com`
2. Go to: https://affiliate-program.amazon.com
3. Sign up → use the email alias above
4. Website URL: `https://pressurewasherpro.com`
5. App type: Website
6. Description: Pressure washer equipment reviews and buying guides
7. After approval: get your Associate tag (usually `yourname-20`)
8. **Replace `REPLACE_WITH_YOUR_AMAZON_TAG` in `page_manifest.json`** with your tag
9. Run `python3 generate_site.py` again to regenerate all pages with live links

## Regenerating content

```bash
# Regenerate all pages (DeepSeek API called)
/Users/lee/.local/venvs/lfi/bin/python3 generate_site.py

# Regenerate one page only
/Users/lee/.local/venvs/lfi/bin/python3 generate_site.py --page best-electric-pressure-washers

# Dry run (scaffold only, no API calls)
/Users/lee/.local/venvs/lfi/bin/python3 generate_site.py --dry-run
```

## Adding more pages

Edit `page_manifest.json` → add entries to the `pages` array → run generator.
Pages cost ~$0.001 each to generate (DeepSeek V4 Flash). 100 pages ≈ $0.10.

## SEO next steps (after live)

1. Submit `sitemap.xml` to Google Search Console
2. Submit to Bing Webmaster Tools
3. Add a basic schema.org `WebPage` + `Review` markup to review pages (generator update)
4. Add internal links from the business guide back to equipment roundups
5. Consider 15–20 long-tail "how to" pages once the core 13 rank
