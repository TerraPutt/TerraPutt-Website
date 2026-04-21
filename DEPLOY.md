# TerraPutt — Deploy

Static site, deployed to Netlify. Form submissions from [follow.html](follow.html) go to Netlify Forms.

## Layout

| Path | Purpose |
|---|---|
| `terraputt-hero-rev-b.html` | Hero (served at `/`) |
| `follow.html` | "Follow the build" signup form |
| `netlify.toml` | Publish config + root redirect + security headers |

## First-time deploy

1. **Push to GitHub**
   ```bash
   git remote add origin git@github.com:<you>/terraputt.git
   git push -u origin main
   ```

2. **Connect to Netlify**
   - netlify.com → Add new site → Import an existing project → pick the repo
   - Build command: *(none)*
   - Publish directory: `.`
   - Deploy. You'll get a `*.netlify.app` subdomain immediately.

3. **Custom domain (Namecheap)**
   - In Netlify: Site settings → Domain management → Add custom domain → enter your domain
   - Netlify will show you either nameservers (easiest) or A/CNAME records
   - **Recommended**: point Namecheap nameservers at Netlify
     - Namecheap → Domain List → Manage → Nameservers → "Custom DNS"
     - Paste the 4 Netlify nameservers (e.g., `dns1.p0X.nsone.net`, etc.)
     - Netlify handles SSL automatically (Let's Encrypt) once DNS propagates
   - Propagation: usually 5–30 min, up to 48h worst case

## Form submissions

The signup form on [follow.html](follow.html) is wired to Netlify Forms:
- `data-netlify="true"` + hidden `form-name` field = auto-detected on first deploy
- Submissions show up in Netlify dashboard → Forms → `follow-build`
- Honeypot field (`bot-field`) filters bots; Netlify's built-in spam filter catches the rest
- Email notifications: Site settings → Forms → Form notifications → add email

Free tier: 100 submissions/month. Past that, $19/mo for 1000.

## Local preview

```bash
python -m http.server 8000
# open http://localhost:8000/terraputt-hero-rev-b.html
```

Forms don't submit locally (Netlify only intercepts on their infra); test form wiring after first deploy.
