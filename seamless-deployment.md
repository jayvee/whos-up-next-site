# Seamless Deployment: Hosting Options for whosupnext.today

Research into replacing the current manual Cloudflare Pages + Wrangler setup with a push-to-deploy static hosting solution.

## Requirements

- Plain static HTML/CSS site (no build step, no framework)
- Push to `main` on GitHub = automatic deploy
- Custom domain: `whosupnext.today`
- HTTPS required (App Store links to privacy policy)
- Commercial use (marketing site for a paid iOS app)
- Ideally simple enough to manage entirely from CLI / Claude
- Open to moving off Cloudflare entirely

---

## The Cloudflare Problem

Every option below that uses Cloudflare DNS requires setting the proxy to **"DNS only" (grey cloud)**. Cloudflare's orange-cloud proxy breaks SSL certificate provisioning on all of these hosts. This means you lose Cloudflare's CDN caching — the main reason to use Cloudflare beyond DNS. At that point, Cloudflare is just an expensive domain registrar.

---

## Option 1: GitHub Pages (Recommended)

**Cost: Free (public repo) or $4/month (private repo via GitHub Pro)**

The simplest option. GitHub Pages is built into GitHub — there's no separate service to sign up for, no dashboard to configure, no CLI tools to install.

### Setup
1. Go to repo **Settings > Pages**
2. Set source to `main` branch, `/ (root)` folder
3. Add a `.nojekyll` file to the repo root (empty file, tells GitHub to skip Jekyll)
4. Save

That's it. Every push to `main` auto-deploys.

### Custom Domain
1. In Settings > Pages, enter `whosupnext.today` as custom domain (auto-creates a `CNAME` file in repo)
2. In Cloudflare DNS, add four A records (DNS only / grey cloud):
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`
3. HTTPS is automatic via Let's Encrypt (once DNS propagates)

### Pros
- Zero config — no `wrangler.toml`, no `netlify.toml`, no `vercel.json`
- Auto-deploy is the default, not a feature to enable
- GitHub's own CDN (Fastly) serves the site — fast globally
- No commercial use restrictions
- Rock-solid reliability

### Cons
- **Private repo requires GitHub Pro ($4/month)** — public repo is free
- No server-side redirects or custom headers
- 100 GB/month bandwidth (soft limit, plenty for a marketing site)
- 10 builds/hour limit
- Must use grey-cloud DNS if keeping Cloudflare

### Verdict
Best balance of simplicity and reliability. The `$4/month for private repo` question is the main decision point. If the repo can be public, this is a no-brainer.

---

## Option 2: Netlify

**Cost: Free (300 credits/month)**

The most polished developer experience for static sites. Smooth GitHub integration, deploy previews on PRs, instant rollbacks.

### Setup
1. Sign up at netlify.com with GitHub
2. Click "Add new site" > "Import an existing project" > select repo
3. Leave build command blank, set publish directory to `/`
4. Click "Deploy site"

Every push to `main` auto-deploys. PRs get preview URLs automatically.

### Custom Domain
1. In Netlify: Site settings > Domain management > Add custom domain
2. In Cloudflare DNS: Add CNAME record `@` → `your-site.netlify.app` (DNS only / grey cloud)
3. HTTPS is automatic via Let's Encrypt

### Pros
- Excellent GitHub integration with deploy previews
- Supports `_redirects` and `_headers` files
- Instant rollbacks to any previous deploy
- No commercial use restrictions
- Can optionally move DNS to Netlify entirely

### Cons
- **Credit-based pricing since Sept 2025**: 300 credits/month shared across bandwidth + deploys
  - ~20 deploys/month + ~30 GB bandwidth before credits run out
  - When credits run out, **site goes down** until next billing cycle
- Must use grey-cloud DNS if keeping Cloudflare
- More constrained free tier than the others

### Verdict
Great DX but the credit system is a concern. For a low-traffic marketing site with infrequent updates, 300 credits is probably fine. But the "site goes down when credits run out" policy is risky.

---

## Option 3: Render

**Cost: Free (Hobby tier)**

Simple, clean platform. Less well-known than Netlify/Vercel but solid for static sites.

### Setup
1. Sign up at render.com
2. Click "New" > "Static Site" > select GitHub repo
3. Leave build command blank, set publish directory to `.`
4. Click "Create Static Site"

Every push to `main` auto-deploys.

### Custom Domain
1. In Render: Settings > Custom Domains > Add
2. In Cloudflare DNS: Add CNAME `@` → `your-site.onrender.com` (DNS only / grey cloud)
3. HTTPS is automatic via Let's Encrypt

### Pros
- Genuinely free, no credit card required
- Auto-deploy on push, no config needed
- 100 GB/month bandwidth
- 500 build minutes/month
- No commercial use restrictions

### Cons
- **Only 2 custom domains on free tier** — root + www uses both slots
  - If you ever need a second domain, must upgrade to Pro ($19/month)
- Must delete any AAAA (IPv6) DNS records — Render doesn't support IPv6
- Must use grey-cloud DNS if keeping Cloudflare
- Less mature ecosystem than Netlify/Vercel

### Verdict
Solid free option with generous limits. The 2 custom domain cap is annoying but workable for a single-site use case.

---

## Option 4: Vercel

**Cost: $20/month (commercial use requires Pro plan)**

Excellent platform, but the free tier explicitly prohibits commercial use — and a marketing site for a paid app qualifies as commercial.

### Setup
1. Sign up at vercel.com with GitHub
2. "Add New" > "Project" > select repo
3. Framework preset: "Other", build command: empty (override), output: `.`
4. Click "Deploy"

Every push to `main` auto-deploys.

### Custom Domain
1. In Vercel: Project Settings > Domains > Add `whosupnext.today`
2. In Cloudflare DNS: Add A record `@` → `76.76.21.21` (DNS only / grey cloud)
3. HTTPS is automatic via Let's Encrypt

### Pros
- Excellent developer experience
- 100 GB/month bandwidth
- 100 deploys/day
- Deploy previews on PRs
- Can host DNS on Vercel directly

### Cons
- **$20/month required** — Hobby plan prohibits "advertising the sale of a product or service"
- Must use grey-cloud DNS if keeping Cloudflare (Vercel actively discourages Cloudflare proxy)
- Optimised for Next.js; plain static sites feel like an afterthought

### Verdict
Great platform, wrong price point. $20/month for a static marketing site is hard to justify when GitHub Pages does it for free.

---

## Option 5: Surge.sh (Not Recommended)

**Cost: $30/month for HTTPS on custom domains**

### Why Not
- No HTTPS for custom domains on free tier — $30/month for SSL is absurd in 2026
- No built-in GitHub integration — requires DIY GitHub Actions
- Single maintainer, documented multi-day outages (3+ days in April 2025)
- Maintainer actively discourages using Cloudflare proxy with Surge
- No rollbacks, no deploy previews, no server-side redirects on free tier

---

## Comparison Table

| | GitHub Pages | Netlify | Render | Vercel | Surge |
|---|---|---|---|---|---|
| **Monthly cost** | Free / $4 | Free | Free | $20 | $30 |
| **Auto-deploy on push** | Yes | Yes | Yes | Yes | No (needs GH Actions) |
| **Setup steps** | 3 | 4 | 4 | 4 | 5+ |
| **HTTPS (custom domain)** | Free, auto | Free, auto | Free, auto | Free, auto | $30/month |
| **Bandwidth** | 100 GB | ~30 GB (credits) | 100 GB | 100 GB | Unstated |
| **Deploy previews** | No | Yes | Yes | Yes | No |
| **Redirects/headers** | No | Yes | No | Yes | $30/month |
| **Commercial use** | Allowed | Allowed | Allowed | $20/month | Allowed |
| **Reliability** | Excellent | Good | Good | Excellent | Poor |
| **Works with CF proxy** | No | No | After setup | No | No |

---

## Moving Off Cloudflare Entirely?

If you transfer DNS away from Cloudflare, the setup gets even simpler for some options:

- **Netlify DNS**: Netlify can manage your domain's DNS directly. Point nameservers to Netlify and everything (DNS + CDN + hosting + HTTPS) is in one place. No grey-cloud/orange-cloud confusion.
- **Vercel DNS**: Same idea, but requires the $20/month Pro plan for commercial use.
- **GitHub Pages**: DNS stays wherever your registrar is. If you transfer the domain to a registrar like Namecheap or Google Domains, you just set A records there.

Domain transfer from Cloudflare is straightforward — unlock the domain, get an auth code, transfer to a new registrar. Takes a few days to process.

**However**, keeping Cloudflare as the registrar and DNS provider (with grey cloud) is perfectly fine for all options. You only lose the CDN proxy, which these hosts replace with their own CDN anyway.

---

## Recommendation

**GitHub Pages** is the strongest choice for this project:

1. **Simplest setup** — 3 steps in the repo settings, no external service to sign up for
2. **Zero maintenance** — no CLI tools, no tokens, no config files (beyond `.nojekyll`)
3. **Push = deploy** — the default behavior, not something to configure
4. **Free for public repos** — or $4/month for private, which you likely already pay for GitHub Pro
5. **Battle-tested reliability** — GitHub's infrastructure, Fastly CDN
6. **No commercial use restrictions**
7. **Works fine with Cloudflare DNS** — just use grey cloud on the A records

The only real trade-off is no deploy previews and no `_redirects` support, neither of which matters for a simple marketing site.

### To migrate

1. Delete the existing Cloudflare Pages project (if any remains)
2. In Cloudflare DNS, replace existing records with the four GitHub Pages A records (DNS only)
3. Add `.nojekyll` file to repo root
4. Enable Pages in repo Settings > Pages (source: `main`, root: `/`)
5. Set custom domain to `whosupnext.today`
6. Wait for HTTPS certificate (minutes to an hour)
7. Update CLAUDE.md to remove Wrangler references
