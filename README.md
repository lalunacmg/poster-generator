La Luna Social Poster Studio — MVP v0.2

A browser-based daily social media poster generator for La Luna Grill + Cafe.

What this version does

Locks every generated artwork to 1080 × 1350 (4:5 portrait) for social-feed posters.

Uses a La Luna visual system inspired by the supplied loyalty poster and the existing La Luna web palette: dark coffee tones, warm orange, cream, green, and gold linework.

Includes the current 40-item La Luna menu fallback so the app works even when the live source cannot be reached.

Tries to sync the menu through /api/menu from https://lalunaorderingapp.vercel.app/.

Falls back to the public La Luna web menu source if the ordering page does not expose a readable menu structure.

Automatically chooses a daily feature with category balancing and a configurable no-repeat window.

Offers rotation modes: Balanced, Bestsellers, Food only, Drinks only.

Creates a 7-day poster queue preview.

Generates a branded poster on an HTML canvas and exports it as a PNG.

Generates an editable social caption with category-aware copy and hashtags.

Lets you upload a product photo per menu item and remembers it in the current browser.

Provides three related layouts: Classic Gold, Ember Night, and Cafe Cream.

Mobile experience

The studio is now optimized for phones as well as desktop:

Mobile bottom navigation separates Poster, Item, Design, and Queue so the long editor does not become one giant scroll.

Inputs use mobile-safe sizing to avoid unwanted iOS zoom and all key touch targets are at least ~44 px tall.

The 4:5 canvas scales to the available phone width while the exported file remains 1080 × 1350.

Safe-area padding supports notched iPhones and Android devices with gesture navigation.

Share uses the native mobile share sheet when Web Share file support is available; otherwise it falls back to PNG download.

The project includes a web app manifest and La Luna app icons so supported browsers can add the studio to the home screen.

Landscape phones get a height-constrained poster preview rather than an oversized canvas.

The supplied official La Luna logo is locked into every poster layout; if the logo cannot load, export is stopped rather than producing an unbranded poster.

The product-photo picker can open the rear camera on supported phones.

Run locally

Requires Node.js 18+.

npm start

Then open:

http://localhost:3000

You can also open index.html directly. The poster generator and bundled menu will work, but live menu sync requires the local server or a deployed serverless endpoint.

Deploy to Vercel

This folder is ready to deploy as a small Vercel project.

Create a new Vercel project from this folder/repository.

No framework preset is required; the root static files are served directly.

api/menu.js becomes the menu-sync serverless endpoint automatically.

Optional: set ALLOWED_MENU_HOSTS to add additional approved menu-source hostnames, comma-separated.

Menu-sync behavior

The serverless menu adapter tries, in order:

The configured La Luna ordering URL.

JSON/Next.js application data embedded in the page.

A recognizable const MENU = [...] structure.

Same-host JavaScript chunks linked by the page.

The public La Luna website menu source as fallback.

The menu parser normalizes different shapes into:

{
  "id": "24",
  "cat": "Coffee",
  "icon": "☕",
  "name": "Spanish Latte",
  "desc": "Rich espresso blended with sweetened condensed milk.",
  "p": 135,
  "pop": true,
  "variants": [
    {"label": "Hot", "price": 135},
    {"label": "Iced", "price": 145}
  ]
}

If the Vercel ordering app uses a separate private API that is not discoverable from its HTML/JavaScript, point the adapter to that JSON endpoint or extend lib/menu-source.js with its exact API route.

Daily selection logic

The default Balanced categories mode rotates across menu categories using the calendar date as a deterministic seed. It excludes menu items used during the configured no-repeat window whenever enough alternatives exist. Today's choice is stored in local browser history so refreshing the page does not unexpectedly change the day's artwork.

Product photos

The menu source currently may not provide image URLs. Upload a photo from the Featured Item section. The app resizes it in-browser and stores a compressed copy in localStorage, then uses it in the poster's hero circle. If no product photo exists, the poster uses a branded category illustration/emoji placeholder.

Phase 2 candidates

The MVP intentionally stops at poster + caption generation. Logical next steps are:

Connect an exact menu API so sync is guaranteed rather than inferred from the public page.

Add a small image library per menu item instead of browser-only uploads.

Add daily scheduled generation in a persistent backend.

Add Meta Graph API integration for Facebook Page and Instagram Business posting after page permissions/tokens are configured.

Add approval workflow: auto-generate every morning → owner approves → publish on schedule.

Add campaign types such as promos, loyalty reminders, sold-out notices, events, and holiday posters.

Files

index.html                         App UI
styles.css                         Responsive desktop + mobile studio styling
app.js                             Daily picker, poster renderer, captions, export
fallback-menu.js                   Bundled 40-item menu
standalone.html                    Single-file mobile-ready version
api/menu.js                        Vercel menu sync endpoint
lib/menu-source.js                 Menu extraction + normalization
server.js                          Zero-dependency local development server
vercel.json                        Vercel configuration
manifest.webmanifest               Mobile/PWA metadata
assets/laluna-official-logo-transparent.png  Locked official poster logo
assets/app-icon-192.png            PWA/home-screen icon
assets/app-icon-512.png            PWA/home-screen icon
assets/brand-reference-poster.png  Supplied visual reference
