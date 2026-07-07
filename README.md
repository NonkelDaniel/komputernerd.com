# komputernerd.com

The website for **Komputernerd BV** — independent computer consultancy by Daniel Mir Heydari.

A single static HTML page: logo, a short intro, a "Trusted by" row, and a contact form.
No build step, no framework.

## Structure

```
index.html      → the whole site (styles are inline)
thanks.html     → shown after the contact form is submitted
assets/         → logo + favicons
CNAME           → custom domain (komputernerd.com)
.nojekyll       → tells GitHub Pages to serve files as-is (no Jekyll processing)
.github/workflows/deploy.yml → auto-deploys to GitHub Pages on every push to main
```

## Editing

Everything is in `index.html`. Common tweaks:

- **Text / intro** — edit the `<header>` block.
- **Trusted by** — edit the `.trusted` block (currently text wordmarks; drop image
  logos into `assets/` and swap the `<span>`s for `<img>` if you prefer).
- **Contact form** — see below.

## Contact form

The form uses [FormSubmit](https://formsubmit.co) — a free service that forwards
submissions to your email, with no account or backend needed. It's already
**activated**, and the form `action` uses FormSubmit's hashed endpoint (rather
than the naked email address) so the address isn't exposed in the page source:

```html
<form action="https://formsubmit.co/<hash>" method="POST">
```

To point it at a different inbox, generate a new hash for that address from the
FormSubmit dashboard (or use the plain `https://formsubmit.co/YOUR-EMAIL` form,
which re-triggers a one-time confirmation email).

## Deployment

Pushing to `main` (e.g. merging a pull request) triggers
`.github/workflows/deploy.yml`, which publishes the site to GitHub Pages.

**First-time setup (once):**

1. In the repo, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **GitHub Actions**.
3. Push to `main` (or re-run the workflow) — the site deploys.

## Pointing the domain (komputernerd.com)

The `CNAME` file already tells GitHub Pages to serve the site at `komputernerd.com`.
To finish, add these DNS records at your domain registrar:

**Apex domain `komputernerd.com`** — four `A` records to GitHub Pages:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

(Optionally add the matching `AAAA` records for IPv6:
`2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.)

**`www.komputernerd.com`** — a `CNAME` record pointing to `<your-github-username>.github.io`.

Then in **Settings → Pages**, set the custom domain to `komputernerd.com` and
enable **Enforce HTTPS** once the certificate is issued (can take a little while).
