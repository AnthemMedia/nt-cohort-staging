# Northside Tutoring — Cohort Landing (Staging)

Static landing page for the cohort program with embedded Retell AI chat widget. Deployable to Vercel as a staging preview.

## What's in here

- `index.html` — the cohort landing page with the Retell chat widget script embedded
- `vercel.json` — minimal Vercel config (clean URLs, blocks search engines from indexing the staging site)

## Before you deploy

Open `index.html` and find this block near the top:

```html
<script
  id="retell-widget"
  src="https://dashboard.retellai.com/retell-widget.js"
  type="module"
  data-public-key="REPLACE_WITH_RETELL_PUBLIC_KEY"
  data-agent-id="REPLACE_WITH_CHAT_AGENT_ID"
  ...
></script>
```

Replace the two placeholders:

1. **`data-public-key`** — Get from Retell Dashboard → Keys → "+ Add Key" → select Public Key. **When you create the key, set the allowed domain to your Vercel staging URL** (e.g. `nt-cohort-staging.vercel.app`). You can add `northsidetutoring.com` later when you go to production.
2. **`data-agent-id`** — Get from your chat agent in the Retell dashboard.

If your agent is currently a voice agent, click the three-dot menu (···) on it in the builder and convert it to a chat agent first.

## Deploy options

### Option 1 — Drag-and-drop (fastest, no account setup beyond signup)

1. Zip this folder
2. Go to [vercel.com/new](https://vercel.com/new)
3. Sign in (free), drag the folder onto the page
4. Vercel hands you a URL like `nt-cohort-staging-xxxxx.vercel.app`
5. Send to Robert

### Option 2 — Vercel CLI (best for iterating)

```bash
npm i -g vercel
cd nt-cohort-staging
vercel        # follow prompts, picks "Other" framework, deploys to preview URL
vercel --prod # when ready, promote to the staging "production" URL
```

### Option 3 — GitHub-connected (recommended once it's stable)

1. Push this folder to a GitHub repo
2. In Vercel, "Import Project" → select the repo
3. Every push to `main` redeploys; every PR gets its own preview URL
4. This is the proper staging workflow when you start iterating on copy/design

## Critical: Retell domain restriction

Retell public keys are scoped to a specific domain. If you deploy and the widget doesn't appear:

- Check the browser console (F12 → Console). If you see auth errors, the public key isn't allowed on the staging URL.
- Go back to Retell Dashboard → Keys → edit the public key → add the Vercel URL to allowed domains.

You can either:
- Use **one key** with both `nt-cohort-staging.vercel.app` and `northsidetutoring.com` allowed, or
- Use **two separate keys** (one staging, one prod) and swap the value before going live — cleaner for tracking usage separately.

## When ready for production

1. Remove the staging banner: in `index.html`, delete the `<div class="staging-banner">` line and remove `class="has-banner"` from `<body>`.
2. Remove `"X-Robots-Tag": "noindex, nofollow"` from `vercel.json` so the page can be indexed.
3. Swap to your production Retell public key (or update domain restrictions on the existing key to include the live URL).
4. Point your domain at Vercel (or copy the HTML into your existing site if NT's main site lives on a different platform).
