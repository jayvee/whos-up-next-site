# Deploy to Cloudflare Pages

Deployment is automatic — push to `main` on GitHub and Cloudflare Pages picks it up.

## Steps

1. Commit your changes
2. Push to `main`:
   ```
   git push origin main
   ```

3. The site will auto-deploy to:
   - **Production:** https://whosupnext.today
   - **Pages URL:** https://whosupnext-site.pages.dev

## Notes

- No build step required — this is a plain static HTML/CSS site
- No manual Wrangler deploy needed — GitHub integration handles it
- Custom domain `whosupnext.today` is configured in the Cloudflare dashboard
