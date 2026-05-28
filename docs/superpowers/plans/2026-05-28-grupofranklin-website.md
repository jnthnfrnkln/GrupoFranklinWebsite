# Grupo Franklin Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a minimal 3-page static website for Grupo Franklin (Private Investment Firm) to www.grupofranklin.com via GitHub Pages.

**Architecture:** Pure HTML/CSS/JS — three standalone HTML pages sharing one stylesheet. No build tools, no frameworks, no dependencies except a Google Fonts import and the Web3Forms API for contact form delivery.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JS, Web3Forms (free contact form API), GitHub Pages (free hosting), Let's Encrypt TLS (auto-provisioned by GitHub).

> **Note on testing:** This is a static site with minimal JS. "Tests" are browser verification steps. The only testable JS behavior is (a) the investor login always shows the error message and (b) the contact form submits to Web3Forms and shows success/error feedback.

> **Web3Forms prerequisite:** Before Task 4, retrieve your Web3Forms access key from https://web3forms.com (Dashboard → Access Keys). You'll paste it into contact.html.

---

## File Map

| File | Responsibility |
|------|---------------|
| `style.css` | Shared CSS: custom properties (colors), reset, nav, footer, mobile hamburger |
| `index.html` | Landing page: hero with rotating GIF, heading, subtitle |
| `contact.html` | Contact page: two-column layout, Web3Forms contact form, email display |
| `investors.html` | Investor portal: fake login card, always shows error on submit |
| `CNAME` | Tells GitHub Pages to serve under www.grupofranklin.com |
| `.gitignore` | Excludes brainstorming session files from git |

---

## Task 1: Initialize git and connect to GitHub

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Initialize git in the working directory**

```bash
git init "G:\My Drive\GF\GF Website"
cd "G:\My Drive\GF\GF Website"
git remote add origin https://github.com/jnthnfrnkln/GrupoFranklinWebsite.git
```

- [ ] **Step 2: Create `.gitignore`**

Create `G:\My Drive\GF\GF Website\.gitignore` with this content:

```
.superpowers/
```

- [ ] **Step 3: Verify remote is set**

```bash
git remote -v
```

Expected output:
```
origin  https://github.com/jnthnfrnkln/GrupoFranklinWebsite.git (fetch)
origin  https://github.com/jnthnfrnkln/GrupoFranklinWebsite.git (push)
```

- [ ] **Step 4: Commit**

```bash
git add .gitignore
git commit -m "chore: initialize repo"
```

---

## Task 2: Create shared stylesheet

**Files:**
- Create: `style.css`

- [ ] **Step 1: Create `style.css`**

Create `G:\My Drive\GF\GF Website\style.css`:

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap');

:root {
  --bg-primary:   #0d1b2a;
  --bg-secondary: #0a1520;
  --border:       #1e3a5f;
  --text:         rgba(255, 255, 255, 0.88);
  --text-muted:   #4a7aad;
  --gold:         #b8962e;
  --footer-text:  #2a4a6a;
}

*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', sans-serif;
  background: var(--bg-primary);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* ── NAV ── */
nav {
  background: var(--bg-secondary);
  border-bottom: 1px solid var(--border);
  padding: 0 40px;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-logo img {
  height: 28px;
  display: block;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 36px;
  list-style: none;
}

.nav-links a {
  color: var(--text-muted);
  text-decoration: none;
  font-size: 11px;
  letter-spacing: 1.8px;
  text-transform: uppercase;
  transition: color 0.2s;
}

.nav-links a:hover {
  color: var(--text);
}

.nav-links .investors-link a {
  color: var(--gold);
  border: 1px solid var(--gold);
  padding: 6px 16px;
  border-radius: 2px;
  transition: background 0.2s, color 0.2s;
}

.nav-links .investors-link a:hover {
  background: var(--gold);
  color: var(--bg-secondary);
}

/* ── HAMBURGER ── */
.nav-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  cursor: pointer;
  background: none;
  border: none;
  padding: 4px;
}

.nav-toggle span {
  display: block;
  width: 22px;
  height: 2px;
  background: var(--text);
  border-radius: 2px;
}

/* ── MAIN ── */
main {
  flex: 1;
}

/* ── FOOTER ── */
footer {
  background: var(--bg-secondary);
  border-top: 1px solid var(--border);
  padding: 20px 40px;
  text-align: center;
  font-size: 12px;
  color: var(--footer-text);
  letter-spacing: 0.5px;
}

/* ── MOBILE NAV ── */
@media (max-width: 640px) {
  nav { padding: 0 24px; }

  .nav-toggle { display: flex; }

  .nav-links {
    display: none;
    position: absolute;
    top: 64px;
    left: 0;
    right: 0;
    background: var(--bg-secondary);
    border-bottom: 1px solid var(--border);
    flex-direction: column;
    align-items: flex-start;
    padding: 24px;
    gap: 20px;
  }

  .nav-links.open { display: flex; }
}
```

- [ ] **Step 2: Verify the file saved correctly** — open `style.css` and confirm the `:root` block has all seven custom properties.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add shared stylesheet"
```

---

## Task 3: Build landing page

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html`**

Create `G:\My Drive\GF\GF Website\index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Grupo Franklin</title>
  <link rel="stylesheet" href="style.css">
  <style>
    .hero {
      min-height: calc(100vh - 125px);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 48px 24px;
      gap: 20px;
    }

    .hero img {
      width: 130px;
      height: 130px;
      object-fit: contain;
    }

    .hero h1 {
      font-size: 32px;
      font-weight: 300;
      letter-spacing: 6px;
      text-transform: uppercase;
      color: var(--text);
      margin-top: 12px;
    }

    .hero p {
      font-size: 11px;
      letter-spacing: 3.5px;
      text-transform: uppercase;
      color: var(--gold);
    }
  </style>
</head>
<body>

  <nav>
    <a class="nav-logo" href="index.html">
      <img src="Resources/Grupo%20Franklin%20Logo%20White%20no%20Background.png" alt="Grupo Franklin">
    </a>
    <button class="nav-toggle" aria-label="Toggle menu" onclick="toggleNav()">
      <span></span><span></span><span></span>
    </button>
    <ul class="nav-links" id="nav-links">
      <li><a href="index.html">Home</a></li>
      <li><a href="contact.html">Contact</a></li>
      <li class="investors-link"><a href="investors.html">Investors</a></li>
    </ul>
  </nav>

  <main>
    <section class="hero">
      <img src="Resources/logo_vertical_axis_rotation.gif" alt="Grupo Franklin">
      <h1>Grupo Franklin</h1>
      <p>Private Investment Firm</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2026 Grupo Franklin &middot; All rights reserved</p>
  </footer>

  <script>
    function toggleNav() {
      document.getElementById('nav-links').classList.toggle('open');
    }
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html` directly in a browser (double-click the file). Check:
- Navy background loads
- Rotating GIF appears centered
- "Grupo Franklin" heading is white, large, spaced
- "Private Investment Firm" is gold
- Nav shows logo (PNG), Home, Contact, and gold-bordered Investors button
- Footer shows copyright line

On a narrow window (< 640px), the nav links should hide and a hamburger button should appear. Click it — links should drop down.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add landing page"
```

---

## Task 4: Build contact page

**Files:**
- Create: `contact.html`

> **Before this task:** Log in to https://web3forms.com, go to Dashboard → Access Keys, and copy your access key. You will paste it in Step 1 where it says `YOUR_WEB3FORMS_ACCESS_KEY`.

- [ ] **Step 1: Create `contact.html`**

Create `G:\My Drive\GF\GF Website\contact.html` — replace `YOUR_WEB3FORMS_ACCESS_KEY` with your actual key:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Contact — Grupo Franklin</title>
  <link rel="stylesheet" href="style.css">
  <style>
    .contact-section {
      max-width: 860px;
      margin: 0 auto;
      padding: 80px 40px;
      display: grid;
      grid-template-columns: 1fr 1.2fr;
      gap: 72px;
      align-items: start;
    }

    .contact-info h2 {
      font-size: 22px;
      font-weight: 300;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--text);
      margin-bottom: 16px;
    }

    .contact-info p {
      color: var(--text-muted);
      font-size: 14px;
      line-height: 1.75;
      margin-bottom: 36px;
    }

    .contact-detail {
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 14px;
    }

    .contact-detail::before {
      content: '';
      width: 7px;
      height: 7px;
      background: var(--gold);
      border-radius: 50%;
      flex-shrink: 0;
    }

    .contact-detail a {
      color: var(--text);
      text-decoration: none;
      transition: color 0.2s;
    }

    .contact-detail a:hover { color: var(--gold); }

    .contact-form {
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    .contact-form input,
    .contact-form textarea {
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 3px;
      padding: 12px 16px;
      color: var(--text);
      font-family: 'Inter', sans-serif;
      font-size: 14px;
      outline: none;
      transition: border-color 0.2s;
      width: 100%;
    }

    .contact-form input:focus,
    .contact-form textarea:focus { border-color: var(--gold); }

    .contact-form input::placeholder,
    .contact-form textarea::placeholder { color: var(--text-muted); }

    .contact-form textarea {
      resize: vertical;
      min-height: 130px;
    }

    .contact-form button {
      background: var(--gold);
      color: var(--bg-secondary);
      border: none;
      border-radius: 3px;
      padding: 13px;
      font-family: 'Inter', sans-serif;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 2.5px;
      text-transform: uppercase;
      cursor: pointer;
      transition: opacity 0.2s;
    }

    .contact-form button:hover { opacity: 0.85; }
    .contact-form button:disabled { opacity: 0.5; cursor: not-allowed; }

    .form-message {
      display: none;
      font-size: 13px;
      padding: 12px 16px;
      border-radius: 3px;
      line-height: 1.6;
    }

    .form-message.success {
      background: rgba(184, 150, 46, 0.08);
      border: 1px solid rgba(184, 150, 46, 0.35);
      color: var(--gold);
    }

    .form-message.error {
      background: rgba(200, 60, 60, 0.08);
      border: 1px solid rgba(200, 60, 60, 0.35);
      color: #e06060;
    }

    @media (max-width: 640px) {
      .contact-section {
        grid-template-columns: 1fr;
        gap: 40px;
        padding: 48px 24px;
      }
    }
  </style>
</head>
<body>

  <nav>
    <a class="nav-logo" href="index.html">
      <img src="Resources/Grupo%20Franklin%20Logo%20White%20no%20Background.png" alt="Grupo Franklin">
    </a>
    <button class="nav-toggle" aria-label="Toggle menu" onclick="toggleNav()">
      <span></span><span></span><span></span>
    </button>
    <ul class="nav-links" id="nav-links">
      <li><a href="index.html">Home</a></li>
      <li><a href="contact.html">Contact</a></li>
      <li class="investors-link"><a href="investors.html">Investors</a></li>
    </ul>
  </nav>

  <main>
    <div class="contact-section">

      <div class="contact-info">
        <h2>Get in Touch</h2>
        <p>We welcome inquiries from prospective partners, investors, and other interested parties.</p>
        <div class="contact-detail">
          <a href="mailto:contact@grupofranklin.com">contact@grupofranklin.com</a>
        </div>
      </div>

      <form class="contact-form" id="contact-form">
        <input type="hidden" name="access_key" value="YOUR_WEB3FORMS_ACCESS_KEY">
        <input type="hidden" name="subject" value="New message from grupofranklin.com">
        <input type="hidden" name="redirect" value="false">
        <input type="text"  name="name"    placeholder="Your name"          required>
        <input type="email" name="email"   placeholder="Your email address" required>
        <textarea           name="message" placeholder="Your message"       required></textarea>
        <button type="submit" id="submit-btn">Send Message</button>
        <div class="form-message" id="form-message"></div>
      </form>

    </div>
  </main>

  <footer>
    <p>&copy; 2026 Grupo Franklin &middot; All rights reserved</p>
  </footer>

  <script>
    function toggleNav() {
      document.getElementById('nav-links').classList.toggle('open');
    }

    document.getElementById('contact-form').addEventListener('submit', async function (e) {
      e.preventDefault();
      const btn = document.getElementById('submit-btn');
      const msg = document.getElementById('form-message');

      btn.disabled = true;
      btn.textContent = 'Sending…';
      msg.style.display = 'none';
      msg.className = 'form-message';

      try {
        const res = await fetch('https://api.web3forms.com/submit', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
          body: JSON.stringify(Object.fromEntries(new FormData(this)))
        });
        const data = await res.json();

        if (data.success) {
          msg.className = 'form-message success';
          msg.textContent = 'Thank you for your message. We will be in touch shortly.';
          this.reset();
        } else {
          throw new Error(data.message || 'Submission failed');
        }
      } catch {
        msg.className = 'form-message error';
        msg.textContent = 'Something went wrong. Please email us directly at contact@grupofranklin.com';
      } finally {
        msg.style.display = 'block';
        btn.disabled = false;
        btn.textContent = 'Send Message';
      }
    });
  </script>
</body>
</html>
```

- [ ] **Step 2: Open `contact.html` in browser and verify layout**

Check:
- Two-column layout: info on left, form on right
- Email address is displayed and is a `mailto:` link
- All three form fields render (name, email, message textarea)
- Gold "Send Message" button at bottom
- Narrow the window below 640px — columns should stack vertically

- [ ] **Step 3: Test the contact form end-to-end**

Fill in the form with your own name, email, and a test message. Click Send.
- Button should read "Sending…" briefly
- A gold-outlined success message should appear: "Thank you for your message. We will be in touch shortly."
- Check the inbox for jf@grupofranklin.com — Web3Forms should have delivered the submission.

If you see an error instead, verify the `access_key` value in the hidden input matches your Web3Forms dashboard key exactly.

- [ ] **Step 4: Commit**

```bash
git add contact.html
git commit -m "feat: add contact page with Web3Forms form"
```

---

## Task 5: Build investor portal

**Files:**
- Create: `investors.html`

- [ ] **Step 1: Create `investors.html`**

Create `G:\My Drive\GF\GF Website\investors.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Investor Portal — Grupo Franklin</title>
  <link rel="stylesheet" href="style.css">
  <style>
    .portal-section {
      min-height: calc(100vh - 125px);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 48px 24px;
    }

    .login-card {
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 48px 40px;
      width: 100%;
      max-width: 380px;
    }

    .login-card h1 {
      font-size: 18px;
      font-weight: 300;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--text);
      margin-bottom: 10px;
    }

    .login-card > p {
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.65;
      margin-bottom: 32px;
    }

    .login-form {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .login-form input {
      background: var(--bg-primary);
      border: 1px solid var(--border);
      border-radius: 3px;
      padding: 12px 16px;
      color: var(--text);
      font-family: 'Inter', sans-serif;
      font-size: 14px;
      outline: none;
      transition: border-color 0.2s;
      width: 100%;
    }

    .login-form input:focus { border-color: var(--gold); }
    .login-form input::placeholder { color: var(--text-muted); }

    .login-form button {
      background: var(--gold);
      color: var(--bg-secondary);
      border: none;
      border-radius: 3px;
      padding: 13px;
      font-family: 'Inter', sans-serif;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 2.5px;
      text-transform: uppercase;
      cursor: pointer;
      margin-top: 4px;
      transition: opacity 0.2s;
    }

    .login-form button:hover { opacity: 0.85; }

    .login-error {
      display: none;
      background: rgba(184, 150, 46, 0.06);
      border: 1px solid rgba(184, 150, 46, 0.25);
      border-radius: 3px;
      padding: 12px 16px;
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.65;
    }

    .login-error a {
      color: var(--gold);
      text-decoration: none;
    }

    .login-error a:hover { text-decoration: underline; }
  </style>
</head>
<body>

  <nav>
    <a class="nav-logo" href="index.html">
      <img src="Resources/Grupo%20Franklin%20Logo%20White%20no%20Background.png" alt="Grupo Franklin">
    </a>
    <button class="nav-toggle" aria-label="Toggle menu" onclick="toggleNav()">
      <span></span><span></span><span></span>
    </button>
    <ul class="nav-links" id="nav-links">
      <li><a href="index.html">Home</a></li>
      <li><a href="contact.html">Contact</a></li>
      <li class="investors-link"><a href="investors.html">Investors</a></li>
    </ul>
  </nav>

  <main>
    <div class="portal-section">
      <div class="login-card">
        <h1>Investor Portal</h1>
        <p>Access your investor documents and reports.</p>
        <form class="login-form" id="login-form">
          <input type="email"    placeholder="Email address" autocomplete="email"             required>
          <input type="password" placeholder="Password"      autocomplete="current-password"  required>
          <button type="submit">Sign In</button>
          <div class="login-error" id="login-error">
            Incorrect email or password combination. Please contact
            <a href="mailto:investorrelations@grupofranklin.com">investorrelations@grupofranklin.com</a>.
          </div>
        </form>
      </div>
    </div>
  </main>

  <footer>
    <p>&copy; 2026 Grupo Franklin &middot; All rights reserved</p>
  </footer>

  <script>
    function toggleNav() {
      document.getElementById('nav-links').classList.toggle('open');
    }

    document.getElementById('login-form').addEventListener('submit', function (e) {
      e.preventDefault();
      document.getElementById('login-error').style.display = 'block';
    });
  </script>
</body>
</html>
```

- [ ] **Step 2: Open `investors.html` in browser and verify**

Check:
- Centered card on navy background
- "Investor Portal" heading, subtitle in muted blue
- Email and password fields render
- Gold "Sign In" button

- [ ] **Step 3: Test the login behavior**

Enter any email and password. Click Sign In.
- The error message should appear below the button: "Incorrect email or password combination. Please contact investorrelations@grupofranklin.com"
- The email in the error should be a clickable `mailto:` link.
- Nothing should be sent to any server.

- [ ] **Step 4: Commit**

```bash
git add investors.html
git commit -m "feat: add investor portal page"
```

---

## Task 6: Add CNAME and push to GitHub

**Files:**
- Create: `CNAME`

- [ ] **Step 1: Create the `CNAME` file**

Create `G:\My Drive\GF\GF Website\CNAME` with exactly this content (no trailing newline issues — just save the file):

```
www.grupofranklin.com
```

- [ ] **Step 2: Commit and push everything**

```bash
git add CNAME
git commit -m "chore: add CNAME for custom domain"
git push -u origin main
```

If git asks for GitHub credentials, authenticate with your GitHub username (`jnthnfrnkln`) and a personal access token (GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic) → generate with `repo` scope).

- [ ] **Step 3: Enable GitHub Pages in the repo settings**

1. Go to https://github.com/jnthnfrnkln/GrupoFranklinWebsite/settings/pages
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**
5. Under **Custom domain**, type `www.grupofranklin.com` and click **Save**

GitHub will start a Pages deployment. It shows a progress indicator — wait for the green checkmark (usually under 2 minutes).

- [ ] **Step 4: Verify GitHub Pages is live on the default domain**

Visit `https://jnthnfrnkln.github.io/GrupoFranklinWebsite` — the landing page should load over HTTPS. (The custom domain won't work yet until Task 7.)

---

## Task 7: Configure Google DNS and verify custom domain

- [ ] **Step 1: Log in to Google Domains and add the CNAME record**

> **Note:** Google Domains was acquired by Squarespace in 2023. If your domain has been migrated, log in at https://domains.squarespace.com instead of https://domains.google.com. The DNS interface is nearly identical.

1. Go to https://domains.google.com (or https://domains.squarespace.com if migrated)
2. Find `grupofranklin.com` → click **Manage** → **DNS**
3. Under **Custom records**, click **Manage custom records**
4. Add a new record:
   - **Type:** CNAME
   - **Host name:** `www`
   - **Data:** `jnthnfrnkln.github.io`
   - **TTL:** 3600 (or leave default)
5. Save the record

- [ ] **Step 2: Wait for DNS propagation**

DNS changes can take 10 minutes to a few hours. You can check propagation status at https://dnschecker.org — search for `www.grupofranklin.com` (CNAME). Once most locations show `jnthnfrnkln.github.io`, move to the next step.

- [ ] **Step 3: Verify HTTPS on custom domain**

Visit https://www.grupofranklin.com — the landing page should load with a valid TLS certificate (padlock in browser). GitHub Pages provisions the Let's Encrypt certificate automatically once DNS resolves; if you see a cert warning, wait 10–15 more minutes and refresh.

- [ ] **Step 4: Smoke test all three pages on the live domain**

| URL | Expected |
|-----|----------|
| https://www.grupofranklin.com | Landing page: rotating GIF, heading, subtitle |
| https://www.grupofranklin.com/contact.html | Contact form with two-column layout |
| https://www.grupofranklin.com/investors.html | Login card; Submit → error message appears |

- [ ] **Step 5: Test the live contact form**

Submit a test message on the live contact.html page. Confirm Web3Forms delivers it to your inbox.

---

## Done

The site is live at https://www.grupofranklin.com with:
- Landing page (rotating logo, branding)
- Contact page (Web3Forms form → email delivery, `contact@grupofranklin.com` displayed)
- Investor portal (fake login, always redirects to `investorrelations@grupofranklin.com`)
- Free HTTPS via Let's Encrypt
- Zero ongoing hosting cost
