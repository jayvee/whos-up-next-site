# Deploy to Cloudflare Pages

Deploy the static site to Cloudflare Pages using wrangler.

## Steps

1. Run the deploy command:
   ```
   npx wrangler pages deploy /Users/jviner/src/whosupnext-site --project-name whosupnext-site --branch main --commit-dirty=true
   ```

2. The site will be available at:
   - **Production:** https://whosupnext.today
   - **Pages URL:** https://whosupnext-site.pages.dev

## Notes

- No build step required — this is a plain static HTML/CSS site
- Wrangler must be authenticated (`npx wrangler login` if needed)
- The `--commit-dirty=true` flag suppresses warnings about uncommitted changes
- Custom domain `whosupnext.today` is configured in the Cloudflare dashboard
