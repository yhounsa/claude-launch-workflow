# Speed Mode — Phase S3: Polish & Ship (15 min)

**Mode:** Speed (`--speed`)
**Time budget:** 15 minutes
**Input required:** Working product from S2
**Output:** Deployed product

---

## Purpose

Run a focused 10-item checklist, fix anything that fails, and provide deployment instructions. No full CRO audit, no launch strategy, no social posts. The goal is a live URL in the next 15 minutes.

---

## Step 1 — Quick CRO checklist

Evaluate each item against the current build. Mark PASS or FAIL.

- [ ] **Primary CTA visible above fold** — On both desktop (1280px) and mobile (375px), the main call-to-action button is visible without scrolling
- [ ] **Mobile responsive** — No horizontal scroll, no broken layouts at 375px width
- [ ] **Page loads < 3 seconds** — Estimate based on bundle size and asset count; run Lighthouse if available
- [ ] **No broken links** — All internal links resolve; no 404s on navigation items or CTAs
- [ ] **Contact / signup form works** — Submit the form; verify the success state appears and data is received
- [ ] **Meta title + description set** — `<title>` and `<meta name="description">` are present and non-generic on every page
- [ ] **OG image set** — `<meta property="og:image">` is present; the image is at least 1200x630px
- [ ] **Favicon set** — A favicon is present and renders in the browser tab
- [ ] **Analytics code present** — Analytics script is in the `<head>` or initialized on app start; at least one event will fire on page view
- [ ] **SSL configured** — The deployment target supports HTTPS (virtually all modern platforms do by default — Vercel, Netlify, Fly.io, Railway, etc.)

---

## Step 2 — Fix failures

For each item marked FAIL, fix it now. These are all quick fixes (< 5 minutes each):

| Item | Typical fix |
|---|---|
| CTA not above fold | Reduce hero padding, shorten headline, move CTA up in DOM |
| Mobile layout broken | Add `overflow-x: hidden` on body, fix grid column count, check image widths |
| Page loads > 3 s | Defer non-critical scripts, compress images to WebP, add lazy loading |
| Broken links | Update hrefs, fix route definitions, remove dead navigation items |
| Form not working | Check API endpoint, check env vars, verify success handler |
| Missing meta tags | Add `<title>` and `<meta name="description">` to page head |
| Missing OG image | Use a placeholder OG image (1200x630, brand color + product name as text) |
| Missing favicon | Use a simple SVG favicon or generate one from a letter |
| No analytics | Add the analytics snippet to the layout component |
| SSL not configured | Choose a deployment platform that provides SSL automatically (Vercel, Netlify, Render) |

Do not over-engineer fixes. The fastest acceptable fix is the right fix here.

---

## Step 3 — Deploy

Provide deployment instructions matched to the product's stack (from `docs/context.md`):

**Astro / Next.js / SaaS web:**
```
# Vercel (recommended for Astro, Next.js)
npx vercel --prod

# Or Netlify
npx netlify deploy --prod --dir=dist
```

**React Native / Expo:**
```
# Build for preview / TestFlight
eas build --platform ios --profile preview

# Build for Android Play Console
eas build --platform android --profile preview
```

**NestJS / Express API:**
```
# Railway (recommended for simplicity)
railway up

# Or Fly.io
fly deploy
```

**Generic:**
```
# Push to main and let CI/CD deploy
git add -A && git commit -m "Ship: [product name] v1.0" && git push origin main
```

Include any required env var setup steps specific to this product.

---

## Done

Once deployed, share:

```
SHIPPED

URL: [production URL]
Stack: [stack]
Time: [S1 + S2 + S3 elapsed]

To iterate:
- Re-run the launch skill with `--from 10` for full CRO audit and launch strategy
- Re-run the launch skill with `--from 9` to add more features (spec-driven)
```

---

## What this phase intentionally skips

- Full CRO audit (that's Phase 10 in Full mode)
- Launch strategy, social posts, launch calendar
- Post-deploy E2E tests against production URL
- Lighthouse detailed report with recommendations
- App Store / Play Console submission process (use Full or Light mode for app launches)

Speed mode gets you live. Full mode gets you optimized and strategically launched.
