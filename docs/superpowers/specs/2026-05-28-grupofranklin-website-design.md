# Grupo Franklin Website — Design Spec
_2026-05-28_

## Overview

A minimal 3-page static website for Grupo Franklin, a private investment firm. Hosted on GitHub Pages with the custom domain www.grupofranklin.com. Zero ongoing cost.

---

## Tech Stack

- **HTML / CSS / JavaScript** — no frameworks, no build tools. Three standalone `.html` files.
- **Hosting:** GitHub Pages (free)
- **Domain:** www.grupofranklin.com — managed via Google Domains (or Google-migrated Squarespace Domains). A CNAME record pointing to `jnthnfrnkln.github.io` enables the custom domain.
- **Contact form delivery:** [Web3Forms](https://web3forms.com) (free, no monthly limit) — form submits to their endpoint with an access key; they email the submission to the owner. No backend required.

---

## Visual Design

### Color Palette

| Role | Value |
|------|-------|
| Background (deep navy) | `#0d1b2a` |
| Background (darker, footer/header) | `#0a1520` |
| Border / divider | `#1e3a5f` |
| Body text | `#ffffff` at 85–90% opacity |
| Muted text | `#4a7aad` |
| Accent (gold) | `#b8962e` |
| Footer text | `#2a4a6a` |

### Typography
- Font: **Inter** (Google Fonts, free) — clean, modern, legible
- Headings: light weight, wide letter-spacing
- Body: regular weight

### Logo Usage
- **Nav bar:** `Grupo Franklin Logo White no Background.png` — scaled to ~28px height
- **Hero section (landing page only):** `logo_vertical_axis_rotation.gif` — centered, ~120px

---

## Navigation

Persistent top nav bar on all three pages:

| Item | Behavior |
|------|----------|
| Logo (left) | Links to `index.html` |
| Home | Links to `index.html` |
| Contact | Links to `contact.html` |
| Investors | Links to `investors.html` — styled as a gold-bordered button to stand out |

---

## Pages

### 1. Landing (`index.html`)

**Purpose:** Brand presence. Communicate legitimacy and identity at a glance.

**Content:**
- Full-viewport hero section, vertically centered
- Rotating logo GIF (~120px)
- "Grupo Franklin" as the primary heading (white)
- "Private Investment Firm" as the subtitle (gold)
- No other body copy — intentionally minimal

**Footer:** `© 2026 Grupo Franklin · All rights reserved`

---

### 2. Contact (`contact.html`)

**Purpose:** Allow visitors to reach the firm.

**Layout:** Two-column on desktop, stacked on mobile
- **Left column:** Section heading ("Get in Touch"), a brief line of copy, and the display email `contact@grupofranklin.com`
- **Right column:** Contact form

**Form fields:**
- Name (text, required)
- Email (email, required)
- Message (textarea, required)
- Submit button ("Send Message") — gold background

**Form delivery:** Web3Forms free tier. The form `action` points to `https://api.web3forms.com/submit`. A hidden `access_key` field carries the owner's Web3Forms key (obtained via their free sign-up). On success, an inline "Thank you" message replaces the form. On error, an inline error message appears.

**Footer:** `© 2026 Grupo Franklin · All rights reserved`

---

### 3. Investor Portal (`investors.html`)

**Purpose:** Provide a login-gated appearance for investor relations. The login is intentionally non-functional — all submissions show an error directing users to email.

**Content:**
- Centered card, max-width ~380px
- Heading: "Investor Portal"
- Subheading: "Access your investor documents and reports"
- Email field (type="email")
- Password field (type="password")
- "Sign In" button (gold)

**Behavior (client-side JS only):**
- On form submit (any credentials), prevent default, display error message inline below the button:
  > "Incorrect email or password combination. Please contact investorrelations@grupofranklin.com"
- The error message email address is a `mailto:` link.
- No actual authentication. No data is sent anywhere.

**Footer:** `© 2026 Grupo Franklin · All rights reserved`

---

## Responsive Design

- Mobile-first layout. Nav collapses to a hamburger menu on small screens.
- Contact page columns stack vertically on mobile.
- Investor card fills full width on mobile with padding.

---

## GitHub Pages Setup

1. Repo: https://github.com/jnthnfrnkln/GrupoFranklinWebsite (already created)
2. Push the site files to the `main` branch
3. Enable GitHub Pages in repo Settings → Pages → Source: `main` / `/(root)`
4. Add a `CNAME` file to the repo root containing: `www.grupofranklin.com`
5. In Google Domains (or Squarespace Domains if migrated), add a CNAME record: `www` → `jnthnfrnkln.github.io`
6. GitHub Pages will provision a free TLS certificate automatically via Let's Encrypt

---

## File Structure

```
/
├── index.html
├── contact.html
├── investors.html
├── CNAME
├── style.css           (shared styles)
├── Resources/
│   ├── Grupo Franklin Logo White no Background.png
│   └── logo_vertical_axis_rotation.gif
└── docs/
    └── superpowers/specs/
        └── 2026-05-28-grupofranklin-website-design.md
```

---

## Out of Scope

- CMS or content management
- Blog or news section
- Analytics (can be added later via Google Analytics or Plausible with a single script tag)
- Actual investor authentication
