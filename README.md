# Dr. Hemlata Moturi, MD — Website

Static one-page brochure site for Dr. Hemlata Moturi, MD (Endocrinologist, Allegheny Endocrinology Associates, PC). Design adapted from drsirishaobgyn.com.

- **Stack**: plain HTML/CSS/JS — no build step
- **Host**: Cloudflare Pages (free tier)
- **CI/CD**: GitHub Actions deploys `public/` on every push to `main`

## Local preview

```bash
cd dr-hemlata-moturi
python3 -m http.server -d public 8080
# open http://localhost:8080
```

## Project layout

```
public/
  index.html         # the whole site (CSS inlined)
  favicon.svg
  images/
    dr-moturi.svg    # placeholder portrait — swap in a real JPG
.github/workflows/
  deploy.yml         # Cloudflare Pages deployment
```

## Replacing the doctor's photo

Drop a real photo into `public/images/` as `dr-moturi.jpg` (or `.png`/`.webp`), then change the two `src` references in `public/index.html`:

```html
<img class="hero-portrait" src="/images/dr-moturi.jpg" ...>
<img class="why-photo"     src="/images/dr-moturi.jpg" ...>
```

Suggested specs: 800×1000+ portrait orientation, neutral background, professional headshot.

## Editing copy

The site has placeholder bio prose and a placeholder services list. Look for these sections in `public/index.html`:

- Hero subhead — `<p class="hero-sub">`
- About — `<section class="sec" id="about">`
- Services cards — `<section class="care-sec" id="services">`
- Quote — `<blockquote class="quote-text">`
- Conditions Treated — `<section class="cond-sec" id="conditions">`

## Deployment — Cloudflare Pages

The GitHub Action in `.github/workflows/deploy.yml` deploys `public/` to a Cloudflare Pages project named **`dr-hemlata-moturi`** on every push to `main`.

### One-time setup

**1. Create the Cloudflare Pages project**

   - Sign in at https://dash.cloudflare.com
   - Workers & Pages → Create → Pages → Create using direct upload
   - Project name: `dr-hemlata-moturi`
   - You can upload a placeholder file just to create the project; the GitHub Action will replace it on first deploy.

**2. Get your Cloudflare Account ID**

   On the Cloudflare dashboard right sidebar, copy **Account ID**.

**3. Create a Cloudflare API token**

   - https://dash.cloudflare.com/profile/api-tokens → Create Token
   - Use the **"Edit Cloudflare Workers"** template, OR create a custom token with:
     - Permissions: `Account → Cloudflare Pages → Edit`
     - Account Resources: include your account
   - Copy the generated token (you'll only see it once).

**4. Add the secrets to your GitHub repo**

   In GitHub: **Settings → Secrets and variables → Actions → New repository secret**

   | Name | Value |
   |---|---|
   | `CLOUDFLARE_API_TOKEN` | the token from step 3 |
   | `CLOUDFLARE_ACCOUNT_ID` | the account ID from step 2 |

**5. Push to main**

   The workflow runs automatically on push. Once green, the site is live at:

   ```
   https://dr-hemlata-moturi.pages.dev
   ```

### Custom domain (optional, later)

Cloudflare Pages → your project → Custom domains → Set up. Cloudflare auto-issues an SSL cert.

## Security note — GitHub token hygiene

The GitHub Personal Access Token used to create this repo was shared in plain text in a chat. **Revoke it now** at https://github.com/settings/tokens and generate a fresh one when you need to push again. Future tokens should never enter a chat or a file — set them as environment variables locally (`export GH_TOKEN=...`) or use `gh auth login`.
