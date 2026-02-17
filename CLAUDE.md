# Who's Up Next? — Website

Marketing website, privacy policy, and support pages for the **Who's Up Next?** iOS app, hosted on Cloudflare Pages at `whosupnext.today`.

## Related Repo

The iOS app lives at `/Users/jviner/src/whos-up-next/`. Key files for context:
- Product description: `product_description.md`
- App Store listing copy: `marketing/app-store/listing_content.md`
- Raw screenshots: `marketing/screenshots/raw/`

You may read files from that repo for reference when updating this site.

## Structure

```
index.html              # Landing page with hero, screenshots, features
privacy/index.html      # Privacy policy (required for App Store)
support/index.html      # Support & FAQ page
screenshots/            # App screenshots (copied from iOS repo)
style.css               # Theme A: Minimal — clean black & white
style-warm.css          # Theme B: Warm — amber/orange social vibe
```

## Themes

Two CSS themes are available. Switch by changing the `<link>` href in the HTML files:
- `style.css` — Minimal: black & white, understated
- `style-warm.css` — Warm: amber/orange tones, coffee-shop vibe

The privacy and support pages currently reference `style.css`. Update all three HTML files when switching themes.

## Deployment

Hosted on **Cloudflare Pages** connected to this GitHub repo. Every push to `main` auto-deploys.
- Domain: `whosupnext.today`
- No build step — plain static HTML/CSS
- Support email: `support@whosupnext.today` (Cloudflare Email Routing → personal Gmail)

## Key URLs

- Privacy policy: `https://whosupnext.today/privacy` — referenced in App Store Connect
- Support: `https://whosupnext.today/support` — referenced in App Store Connect
- Contact: `support@whosupnext.today`

## App Context

Who's Up Next? is an iOS app for social groups to manage purchase rotations fairly using on-device facial recognition. Core selling points for the website:
- Point camera at group, app recognises faces, tells you who's buying next
- Fair rotation algorithm (not random — tracks actual history)
- Multiple independent groups
- 100% on-device, no accounts, no cloud, no tracking
- Camera and contacts permissions only, both optional

## Permissions
- You may freely read files from the iOS app repo at `/Users/jviner/src/whos-up-next/`
- You may freely create, edit, and delete files in this repo
- Do not modify files in the iOS app repo from this session
