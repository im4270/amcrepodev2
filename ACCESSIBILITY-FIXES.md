# AMC Financial Services — Accessibility Fixes

**Standard:** WCAG 2.1 Level AA (as required under the SEBI accessibility circular, IS 17802 and GIGW 3.0)
**Date:** 30 October 2026
**Pages fixed:** 19 (all pages in the shared copy). Each one passes the automated WCAG 2.1 A/AA checks with 0 violations.

---

## Important: read before putting anything live

The pages in this repository were made from a **browser-saved copy** of the website (file names end in `.php.html`). The server-side code is **not** part of that copy, including:

- `contact_includes/process.php` (contact form)
- `subscribe_ajax.php` (subscribe box)
- `contact_includes/captcha.php` (captcha)

**Do not upload these `.php.html` pages over the live website.** If you do, the contact form, subscribe box, captcha and page links will stop working.

**The safe way:** apply the changes below to the real `.php` files on the server. Every change is a small addition, and nothing is removed. Steps 1 and 2 cover almost everything.

---

## Step 1 — Add one CSS file (fixes all colour-contrast issues)

1. Upload `assets/css/accessibility.css` (in this repo) to `assets/css/` on the server.
2. Add this line just before `</head>` on every page, or once in the shared header file if the site uses one:

```html
<link rel="stylesheet" href="assets/css/accessibility.css">
```

For pages inside `services/` and `blogs/`, use `../assets/css/accessibility.css`.

This only changes colours that are too light to read. The layout stays the same:

| Where | Element | Before | After |
|---|---|---|---|
| Home, Services | "services" word in "services We offer" heading | #00D094 on #EEF1F6 (1.77:1) | #008F63 (3.63:1) |
| Home, Services | Paragraph under that heading | #6B777F (4.05:1) | #5A656C (5.28:1) |
| Home | 4 counter captions ("Timely market update", etc.) | faded to 80% (3.16:1) | full opacity (4.6:1+) |
| Blog: Trading Business | "Join Now" button, SEBI registration badge | white on #00C6FF (1.99:1) | white on #0077A8 (5.00:1) |
| Services: Investment Planning, Management | "Important" boxes | #6B777F on #F8FBF9 (4.41:1) | #5F6B72 (5.26:1) |
| Services: Strategy Consulting | Green banner | #00A86B background (2.92:1) | #007A4D (5.12:1) |
| Investor complaints page | "Research Analyst Disclosure" label | #6B777F on #E9FAF5 (4.25:1) | #5A656C (5.54:1) |

Dark mode is not affected.

---

## Step 2 — Give icon-only links a name (WCAG 2.4.4, 4.1.2)

Screen readers announce these links as just "link", because they contain only an icon. Add `aria-label` to the link and `aria-hidden="true"` to the icon. These are in the **shared header/footer** and appear on every page.

**Floating buttons (every page)**
```html
<!-- find -->
<a href="https://t.me/amcfinancial" target="_blank" class="float-btn btn-telegram" data-label="Telegram Desk">
<!-- replace -->
<a href="https://t.me/amcfinancial" target="_blank" class="float-btn btn-telegram" data-label="Telegram Desk" aria-label="Telegram (opens in new tab)">

<!-- find -->
<a href="https://wa.me/8420006286" target="_blank" class="float-btn btn-whatsapp" data-label="WhatsApp Support">
<!-- replace -->
<a href="https://wa.me/8420006286" target="_blank" class="float-btn btn-whatsapp" data-label="WhatsApp Support" aria-label="WhatsApp (opens in new tab)">
```

**Social icons (footer on every page, plus the Contact page)**: add the `aria-label` matching the icon:

| Icon class | Add to the `<a>` |
|---|---|
| `fa-facebook-f` | `aria-label="Facebook"` |
| `fa-instagram` | `aria-label="Instagram"` |
| `fa-youtube` | `aria-label="YouTube"` |
| `fa-linkedin-in` | `aria-label="LinkedIn"` |
| `fa-twitter` | `aria-label="Twitter"` |

Example:
```html
<!-- find -->
<a href="https://www.facebook.com/share/1DS9ShEda9/" class="social__link social__link--style22"><i class="fab fa-facebook-f"></i></a>
<!-- replace -->
<a href="https://www.facebook.com/share/1DS9ShEda9/" class="social__link social__link--style22" aria-label="Facebook"><i class="fab fa-facebook-f" aria-hidden="true"></i></a>
```

**Back-to-top button (every page)**
```html
<!-- find -->
<a href="#" class="scrollToTop scrollToTop--style1"><i class="fa-solid fa-arrow-up-from-bracket"></i></a>
<!-- replace -->
<a href="#" class="scrollToTop scrollToTop--style1" aria-label="Back to top"><i class="fa-solid fa-arrow-up-from-bracket" aria-hidden="true"></i></a>
```

**Video play button (About page)**
```html
<!-- find -->
data-fslightbox=""><i class="fa-solid fa-circle-play"></i></a>
<!-- replace -->
data-fslightbox="" aria-label="Play video"><i class="fa-solid fa-circle-play" aria-hidden="true"></i></a>
```

---

## Step 3 — Image text alternatives (WCAG 1.1.1)

**Service card icons (Home, Services):** these are decorative, because the card title next to them already says what they are. Add an empty `alt=""`:
```html
<img src="assets/images/service/1.png" class="card-img" alt="">
```
Do the same for `service/2.png`, `3.png` and `4.png` (classes `card-img`, `card-img2`, `card-img3`, `card-img4`).

**Captcha image (Contact page)**
```html
<!-- find -->
<img src="contact_includes/captcha.php" id="captchaImage">
<!-- replace -->
<img src="contact_includes/captcha.php" id="captchaImage" alt="CAPTCHA security code - type the characters shown into the box below">
```

---

## Step 4 — Two pages not in the shared copy (WCAG 3.1.1)

`blog-details.php` and `team.php` were not in the copy we received. On both, the `<html>` tag has no language. Change:
```html
<html>   →   <html lang="en">
```

---

## After the changes are live

Tell us once the changes are on the live website. We will re-scan the same 21 pages to confirm, and then issue the compliance documents.
