La Luna Social Poster Studio — v0.3
A mobile-ready daily social media poster generator for La Luna Grill + Cafe.
What changed in v0.3
- /poster is now a locked rendering mode. Every poster generation uses the same cinematic composition system rather than switching between unrelated layouts.
- Added a dedicated Raw Photo → /poster workflow. Choose a menu item, upload an untouched food or drink photo, and the poster is regenerated immediately.
- The raw photo can come from the phone camera or gallery. Desktop users can also drag and drop an image into the upload field.
- Uploaded photos are resized in the browser and remembered per menu item in local storage.
- The renderer now treats the uploaded image cinematically with a full-bleed blurred background, color grading, hero crop, vignette, warm light, film-grain texture, gold framing, and high-contrast typography.
- The old design selector is now a cinematic mood selector: Cinematic Gold, Cinematic Ember, or Cinematic Cream. The composition remains /poster in all three.
- The official La Luna logo is mandatory on every output. If it cannot load, poster export is stopped rather than creating an unbranded image.
- Output remains locked to 1080 × 1350 px (4:5 portrait) for Facebook and Instagram feed posters.
Daily workflow
1. Open the Item tab.
2. Select the menu item to feature.
3. Upload the raw food or drink photo in RAW PHOTO → /POSTER.
4. The app automatically creates the cinematic poster of the day.
5. Optionally adjust the headline, cinematic mood, promo line, or CTA.
6. Open Poster and download or share the finished PNG.
If no raw photo is available, the app can still render a cinematic branded placeholder for the selected item.
Existing automation features
- Includes the current 40-item La Luna menu fallback so the app still works if the live source cannot be reached.
- Tries to sync the menu through /api/menu from https://lalunaorderingapp.vercel.app/.
- Automatically chooses a daily feature with category balancing and a configurable no-repeat window.
- Rotation modes: Balanced, Bestsellers, Food only, Drinks only.
- Generates a 7-day poster queue preview.
- Creates an editable social caption with category-aware copy and hashtags.
- Remembers today's selection so refreshing does not unexpectedly change the featured item.
Mobile experience
- Bottom navigation separates Poster, Item, Design, and Queue.
- Touch targets and form controls are mobile-sized to avoid accidental taps and iOS zoom.
- The poster preview scales to the phone width while exported artwork remains 1080 × 1350.
- Safe-area support works with iPhone notches/home indicators and Android gesture navigation.
- The raw photo field can invoke the rear camera on supported phones.
- Native Share is used where Web Share file support is available; otherwise the app downloads the PNG.
- PWA manifest and icons are included so supported browsers can add the studio to the home screen.
Run locally
Requires Node.js 18+.
npm start
Then open:
http://localhost:3000
You can also open standalone.html directly for the single-file version. Live menu sync requires the local server or the deployed serverless endpoint.
Deploy to Vercel
1. Create a Vercel project from this folder/repository.
2. No framework preset is required.
3. api/menu.js becomes the menu-sync serverless endpoint.
4. Optional: set ALLOWED_MENU_HOSTS to add approved menu-source hostnames, comma-separated.
Poster engine
The app defines /poster as the permanent poster recipe. It is a browser-side renderer and does not require an external AI image API.
For a supplied raw photo, the renderer applies:
- blurred full-bleed background derived from the same photo;
- contrast, saturation, and brightness grading;
- large rounded cinematic hero crop;
- dark lower image fade for readable typography;
- vignette and warm directional glow;
- subtle grain/texture;
- La Luna gold framing and brand palette;
- official La Luna logo;
- menu item, category, starting price, description, promo line, CTA, and ordering URL.
The three moods only modify the grade/accent palette; the /poster composition remains fixed.
Menu-sync behavior
The serverless adapter tries, in order:
1. The configured La Luna ordering URL.
2. JSON/Next.js application data embedded in the page.
3. A recognizable const MENU = [...] structure.
4. Same-host JavaScript chunks linked by the page.
5. The public La Luna website menu source as fallback.
If the ordering app later exposes a dedicated JSON API, update lib/menu-source.js to use that endpoint directly for the most reliable synchronization.
Storage note
Raw photos are compressed and stored in browser localStorage per menu item. Browser storage is intentionally lightweight; for a production multi-device workflow, the next step is a shared image library or cloud storage rather than local-only photo persistence.
Suggested next phase
- Shared product-photo library for all menu items.
- Persistent backend for daily scheduled generation.
- Approval flow: auto-generate → owner approves → publish.
- Meta Graph API integration for Facebook Page and Instagram Business posting.
- Campaign modes for promos, loyalty reminders, events, holidays, and sold-out notices.
Files
index.html                                App UI
styles.css                                Responsive desktop + mobile styling
app.js                                    Daily picker + cinematic /poster renderer
fallback-menu.js                          Bundled 40-item menu
standalone.html                           Single-file mobile-ready version
api/menu.js                               Vercel menu sync endpoint
lib/menu-source.js                        Menu extraction + normalization
server.js                                 Zero-dependency local development server
vercel.json                               Vercel configuration
manifest.webmanifest                      PWA metadata
assets/laluna-official-logo-transparent.png  Locked official poster logo
assets/app-icon-192.png                   PWA/home-screen icon
assets/app-icon-512.png                   PWA/home-screen icon
assets/brand-reference-poster.png         Supplied visual reference
v0.3.1 mobile tab scrolling
On screens up to 820 px wide, Poster, Item, Design, and Queue now use viewport-contained scrolling above the fixed bottom navigation. Each tab keeps its own scroll position while you switch tabs. Programmatic jumps (for example, generating from a raw photo and returning to Poster) intentionally open the destination tab at the top so the newly generated poster is immediately visible. iOS momentum scrolling, safe-area spacing, and overscroll containment are included.
