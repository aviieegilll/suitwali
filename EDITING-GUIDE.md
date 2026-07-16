# Suitwali — Editing Guide

This project is a **single HTML file** (`index.html`) plus an `assets` folder
of images. There is no build step, no server, and no other code files —
everything you'll ever need to edit lives inside `index.html`, which you can
open and edit directly from an iPad text editor (e.g. Textastic, Kodex, or
the Files app + a code editor of your choice) or on desktop.

Every section of the file is labelled with a comment banner like:

```
/* ============================================================
   PRODUCT DATA
   ============================================================ */
```

Use your editor's search function to jump straight to any of these section
names. You will only ever need to touch two of them for day-to-day updates:
**PRODUCT DATA** and, occasionally, **CONFIG**.

---

## 1. How to Replace a Product

Search for `PRODUCT DATA` in `index.html`. You'll find a JavaScript array
called `PRODUCTS`, made up of objects that look like this:

```js
{
  id: "prod-sw-an-001", code: "SW-AN-001", name: "Ivory Pearl Anarkali",
  category: "Anarkali", collection: "Noor",
  colour: "Ivory", fabric: "Katan Silk", handwork: "Aari work with hand-set pearls",
  description: "A boat-neck Anarkali in Katan silk, finished with dense pearl and dabka embroidery along the neckline and cuffs.",
  craftStory: "Aari work uses a hooked needle to chain-stitch at speed and precision, allowing dense pearl and dabka work without weighing down the silk beneath it.",
  customisation: "Made-to-measure available. Sleeve length and neckline depth can be adjusted at no extra cost.",
  delivery: "Ready-to-ship: 3–5 days. Made-to-measure: 3–4 weeks.",
  care: "Dry clean only. Store flat or on a padded hanger, wrapped in muslin to protect the pearl embroidery.",
  mainImage: "assets/products/sw-an-001-main.webp",
  gallery: ["assets/products/sw-an-001-main.webp", "assets/products/sw-an-001-gallery-1.webp", "assets/products/sw-an-001-gallery-2.webp"],
  video: null, featured: true, available: true,
},
```

**To retire an old style and bring in a new one:**
1. Find the product object you want to replace.
2. Change every text field (`name`, `colour`, `fabric`, `handwork`,
   `description`, `craftStory`, `customisation`, `delivery`, `care`) to
   describe the new piece.
3. Give it a **new product code** if it's a genuinely different style — see
   §4 below for the code system. If you're just updating the same style
   with new photos, keep the code as-is.
4. Update `mainImage` and `gallery` to point at the new image files (see §2).
5. Save the file.

**To add a brand-new product without removing one:** copy an entire object
(from the opening `{` to the closing `},`), paste it as a new entry in the
`PRODUCTS` array, give it a unique `id` and `code`, and fill in its details.

**To temporarily mark something out of stock:** change `available: true` to
`available: false`. It will still show in the catalogue with an "Currently
Unavailable" label, rather than disappearing — useful while you restock
rather than losing the product's search/SEO presence.

**To remove a product entirely:** delete its whole object (including the
surrounding `{ ... },`) from the array.

You do **not** need to touch anything else in the file — the product grid,
search, filters, and product detail view all read from this one array
automatically.

---

## 2. How to Replace Images

All images live in `/assets/`, organised by type:

```
assets/
├── logo/         → the Suitwali wordmark
├── hero/          → the homepage hero image
├── collections/    → one image per collection (5 total)
├── products/        → main + gallery images per product
├── craftsmanship/     → About/Craftsmanship section photography
└── icons/              → favicon + social share image
```

**Easiest method — same filename:** if you replace a file with a new one
using the **exact same filename**, you don't need to touch `index.html` at
all. For example, to update the Noor collection photo, replace
`assets/collections/noor.webp` with your new photo, keeping the filename
`noor.webp`.

**Adding a product's images:** each product needs a `mainImage` and 2–3
`gallery` images. Follow the existing naming pattern:
`assets/products/{code-lowercase}-main.webp`,
`assets/products/{code-lowercase}-gallery-1.webp`,
`assets/products/{code-lowercase}-gallery-2.webp`. Upload your files with
those names, and they'll match what the product's data object already
points to (see §1).

**Image format:** use `.webp` for photos (smaller file size, loads faster
on mobile — most phones and export tools support this now). If your camera
or design tool only gives you `.jpg`, that will still work — just update the
file extension in the relevant `mainImage` / `gallery` / `heroImage` line in
`index.html` to match (e.g. `sw-an-001-main.jpg` instead of `.webp`).

**Image size guidance:** aim for roughly 1600px on the longest side for
product/collection photos — large enough to look sharp, small enough to
load quickly on mobile data. The placeholder images currently in the folder
are exactly this size, so matching their dimensions is a safe default.

**The logo:** `assets/logo/suitwali-logo.svg` is currently a simple text
placeholder. Replace it with your real logo file once you have one — SVG is
ideal (crisp at any size), but a PNG with a transparent background will also
work if you rename the file reference in the two spots `suitwali-logo.svg`
appears in `index.html` (header and footer).

---

## 3. How to Change the WhatsApp Number

Search for `CONFIG` in `index.html`. You'll find:

```js
const CONFIG = {
  whatsappNumber: "910000000000",
  studioPhone: "+91 00000 00000 (placeholder)",
  studioEmail: null,
};
```

Replace `"910000000000"` with your real WhatsApp number in **international
format, digits only, no `+`, no spaces**. For an Indian mobile number, that
means: country code `91` followed by the 10-digit number.
Example: a number like `+91 98765 43210` becomes `"919876543210"`.

While you're there:
- `studioPhone` is the number shown in the Contact section — update it to
  match, formatted however you'd like it to display (spaces/dashes are fine
  here, unlike `whatsappNumber`).
- `studioEmail` is currently `null`, meaning the Contact section shows "Not
  yet available." Once you have a studio email, change it to
  `studioEmail: "you@example.com",` (with quotes) and it will appear
  automatically.

---

## 4. Product Code System

Every product code follows the pattern `SW-{TYPE}-{NUMBER}`:

| Category | Code | Example |
|---|---|---|
| Suit | `ST` | `SW-ST-001` |
| Anarkali | `AN` | `SW-AN-001` |
| Lehenga | `LH` | `SW-LH-001` |
| Sharara | `SH` | `SW-SH-001` |
| Bridal | `BR` | `SW-BR-001` |

When adding a new product, use the next unused number for that category
(e.g. if `SW-AN-003` already exists, your next Anarkali is `SW-AN-004`).
**Never reuse a code** once it's been used for a product, even after that
product is retired — this keeps any external references (screenshots,
past WhatsApp conversations, saved links) from ever pointing at the wrong
item later.

---

## 5. How to Update Homepage Text

Most homepage copy (the hero title, the "House of Suitwali" introduction,
About section, testimonials, etc.) is plain text directly inside
`index.html`, in the `<body>` — not inside the JavaScript. Use your editor's
search function to find the words you want to change (e.g. search for
"Handcrafted for the moments" to jump straight to the hero headline) and
edit the text between the HTML tags directly. You don't need to understand
the surrounding HTML — just change the words between `>` and `<`.

The two testimonials (search for `In Her Words`) are placeholder quotes —
replace both the quote text and the `<cite>` attribution line with real
customer feedback once you have it (with their permission).

---

## 6. How to Upload to GitHub and Deploy on Vercel

**One-time setup:**

1. Create a free [GitHub](https://github.com) account if you don't have one.
2. Create a new repository (e.g. named `suitwali`).
3. Upload `index.html` and the entire `assets` folder to that repository —
   GitHub's web interface lets you drag and drop files directly (Add file →
   Upload files), which works fine from an iPad browser.
4. Create a free [Vercel](https://vercel.com) account and choose
   "Import Project" → connect your GitHub account → select the `suitwali`
   repository. Vercel will detect it as a static site automatically — no
   configuration is needed since there's no build step.
5. Click Deploy. Vercel will give you a live URL within a minute or two.
6. In Vercel's project settings, add your custom domain (`suitwali.in`) under
   the Domains tab, and follow Vercel's instructions to point your domain's
   DNS at it.

**Every future update (from an iPad):**

1. Edit `index.html` (and/or replace image files) using your preferred
   iPad editor or directly in GitHub's web-based file editor (open the file
   on github.com, tap the pencil/edit icon, make your change, commit).
2. Commit the change with a short message describing what you updated
   (e.g. "Add new Bridal Couture piece SW-BR-003").
3. Vercel automatically detects the new commit and redeploys the site
   within about a minute — no manual redeploy step required.

That's the entire workflow: edit the file, commit, and Vercel handles the
rest. There's no `npm install`, no build command, and nothing else to run.

---

## What's Deliberately Not Included (and why)

- **No live checkout/payments** — by design, per the brand's enquiry-led
  (not self-checkout) sales model. Every "Enquire on WhatsApp" button leads
  to a real conversation, not a cart.
- **No backend enquiry storage yet** — enquiries go straight to WhatsApp.
  If you later want every enquiry logged somewhere before the WhatsApp
  redirect (e.g. into a spreadsheet or database), that would be a small,
  separate addition (a Vercel serverless function) layered on top of this
  file — flag it if/when you want that, since it's outside this single-file
  scope by design.
- **No separate product/collection URLs** — every product opens in an
  in-page overlay rather than its own page, per the simplified architecture.
  This means individual products can't be linked to directly from outside
  the site (e.g. you can't send someone a link straight to one Anarkali).
  If that becomes something you need later, it's a deliberate trade-off
  that can be revisited.
