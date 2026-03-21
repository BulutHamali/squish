# Squish — Browser-Based Image Compressor

> Compress JPG, PNG, and WebP images instantly. 100% client-side. Your files never leave your browser.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Why This Exists

"Compress image" gets 2M+ searches per month globally. It is one of the most searched utility queries on the entire internet. Every student submitting homework, every HR person processing headshots, every e-commerce seller optimizing product photos, every developer shrinking assets — they all search for this.

TinyPNG, Compressor.io, and iLoveIMG dominate. They all upload your files to their servers. For most users, this is fine. But for anyone compressing confidential screenshots, client work, medical images, or employee photos — server-side processing is a dealbreaker.

This tool compresses images entirely in the browser using Canvas API and WebAssembly. The file never touches a server. The privacy angle is real, and it's the entire competitive positioning.

---

## Business Model

This is a high-volume ad revenue play. No paywall needed.

| Revenue Source | Expected (at 150K visits/mo) |
|---------------|------|
| Google AdSense | $500–$1,500/month |
| Mediavine (at 50K+ sessions) | $1,500–$3,500/month |
| Optional soft paywall (batch mode) | $200–$500/month |

**Why ads work here:** Image compressor users are a broad audience (not just devs), which means diverse ad inventory. Marketers, designers, and business users have high CPMs. The key is ONE ad placement, below the tool, not interfering with the UX.

**Cost to operate:** $0/month. Static site. All processing client-side. No backend. No API costs.

---

## Target Keywords (SEO)

**Primary (monster volume):**
- `compress image` — 400K+/month
- `image compressor` — 300K+/month
- `compress jpg` — 200K+/month
- `compress png` — 150K+/month
- `reduce image size` — 150K+/month
- `compress image online` — 100K+/month

**Long-tail (lower competition, rank faster):**
- `compress image to 100kb` — 80K+/month
- `compress image to 200kb` — 50K+/month
- `compress image to 1mb` — 40K+/month
- `compress jpg without losing quality` — 30K+/month
- `reduce image size to 50kb` — 25K+/month
- `compress image for email` — 20K+/month
- `compress png without losing quality` — 20K+/month
- `reduce photo size for upload` — 15K+/month

**Size-specific pages (each = separate landing page):**
- `/compress-to-100kb` → "Compress Image to 100KB"
- `/compress-to-200kb` → "Compress Image to 200KB"
- `/compress-to-1mb` → "Compress Image to 1MB"
- `/compress-to-50kb` → "Compress Image to 50KB"
- `/compress-to-20kb` → "Compress Image to 20KB"

These are extremely high-intent, specific queries. Each page has the same tool but with the target size pre-filled.

---

## Tech Stack

```
Frontend:    Vanilla HTML + CSS + JS (or lightweight Vite + Preact)
Compression: Browser Canvas API (for JPG quality reduction)
             + Squoosh WASM codecs (for advanced PNG/WebP)
Hosting:     Vercel (static)
Analytics:   Plausible
Ads:         Google AdSense or Mediavine
```

**Why Canvas API is enough for v1:** For JPG compression, the browser's built-in `canvas.toBlob(callback, 'image/jpeg', quality)` method is fast and effective. You adjust the `quality` parameter (0 to 1) and get a smaller file. No external libraries needed for JPG.

For PNG and WebP, use Google's Squoosh WASM codecs for better results. But start with just JPG in v1 — it covers 70%+ of use cases.

**Monthly cost:** $0.

---

## Branding & UI Design Strategy

> **"Make it look like a $50/month SaaS, but keep it free."**
> TinyPNG looks good. Compressor.io is decent. iLoveIMG is cluttered. Your tool needs to look cleaner than all of them while being 100% free and 100% private. The "trust gap" between design quality and price ($0) is what converts.

### Why Design Is Especially Critical for Image Tools

Users who need image compression are often designers, marketers, and content creators — people with high aesthetic standards. If your compressor looks cheap, they'll bounce to TinyPNG. If it looks as good as TinyPNG but adds the privacy story, they'll stay AND come back.

Also: image compressor users are a premium ad audience. Designers and marketers have high CPMs. A beautiful interface keeps them on the page longer = more ad impressions = higher revenue.

### Design System

```
Theme:           Light mode (primary) — clean, professional, minimal
Font (UI):       DM Sans (400, 500, 600, 700) — modern, warm
Font (mono):     Space Mono (400, 700) — for file sizes, percentages, stats
Primary Color:   #3B82F6 → #2563EB (blue gradient) — trust, reliability
Background:      #FFFFFF (pure white)
Surface:         #FAFAFA (gray-50) — for cards and controls
Border:          #F0F0F0 to #E5E5E5 — soft, warm grays
Text Primary:    #171717 (neutral-900)
Text Secondary:  #737373 (neutral-500)
Text Muted:      #A3A3A3 (neutral-400)
Success:         #16A34A (green-600) — for good compression results
Warning:         #D97706 (amber-600) — for marginal compression
Privacy:         #16A34A text on no background — understated but present
```

**Why light mode:** Same reasoning as ClearCut — this is a visual tool handling images. White backgrounds let users see their images clearly and feel "clean" (matching the compression metaphor).

**Why blue:** Blue signals trust and reliability. For a tool that promises "compress without losing quality," blue subconsciously reinforces that promise. It's also neutral enough not to compete with the images being processed.

### Component Library

Use **Shadcn/UI** light theme components or replicate with Tailwind:
- `Card` — for control panel and result display
- `Button` — primary (blue gradient), ghost (outline)
- `Slider` — for quality control (custom-styled range input)
- `ToggleGroup` — for quality/target-size mode switch
- `Badge` — for compression percentage results
- `Dialog` — if adding a paywall later

### Layout: The "Upload → Configure → Compress → Download" Flow

```
┌───────────────────────────────────────────────┐
│  [◆ Squish]          [→50KB][→100KB][→1MB]    │  ← Nav + size shortcuts
├───────────────────────────────────────────────┤
│            h1 + subtitle + privacy note        │
├───────────────────────────────────────────────┤
│                                                │
│  STATE 1: No file                              │
│  ┌──────────────────────────────────┐          │
│  │      [  DROP IMAGE HERE  ]       │          │  ← Big drop zone
│  │      or click to browse          │          │
│  └──────────────────────────────────┘          │
│                                                │
│  STATE 2: File selected                        │
│  ┌──────────────────────────────────┐          │
│  │  [thumb] filename.jpg · 2.4 MB   │          │
│  │                                  │          │
│  │  [Quality slider ●──── ] [Target]│          │  ← Mode toggle + controls
│  │                                  │          │
│  │  [      Compress Image       ]   │          │  ← Full-width CTA
│  └──────────────────────────────────┘          │
│                                                │
│  STATE 3: Result                               │
│  ┌──────────────────────────────────┐          │
│  │  Original    →    Compressed     │          │
│  │  2.4 MB           340 KB        │          │  ← Big numbers
│  │                   −86%           │          │  ← Green badge
│  │                                  │          │
│  │  [↓ Download]  [Compress Another]│          │
│  └──────────────────────────────────┘          │
│                                                │
├───────────────────────────────────────────────┤
│  [Ad: one banner, below tool]                  │
├───────────────────────────────────────────────┤
│  SEO content (below fold)                      │
└───────────────────────────────────────────────┘
```

### Critical UI Rules

1. **Three states, one page.** The tool transitions between Empty → Controls → Result without page navigation. Each state replaces the previous in the same container. Smooth transitions (fade or slide).

2. **The result is the hero moment.** When compression finishes, the before/after comparison must feel satisfying. Use large monospace numbers for file sizes. Use a green badge for the percentage reduction. The visual should communicate "look how much space you saved" instantly.

3. **Mode toggle is prominent.** "Quality slider" vs "Target size" are two fundamentally different user intents. The toggle between them should be a styled segmented control (pill toggle), not a dropdown or radio buttons. Each mode shows different controls below it.

4. **Size presets are in the nav.** Small pills: "→50KB", "→100KB", "→200KB", "→1MB". Clicking one switches to target-size mode AND sets the preset. These are also internal links when built as separate pages (each page = separate SEO keyword).

5. **File thumbnail is visible.** After selecting a file, show a small (48×48) thumbnail of the image alongside the filename and size. This confirms to the user that the right file was selected.

6. **One ad placement maximum.** Below the tool, above the SEO content. Never above the fold. Never in the sidebar. Never a popup.

7. **Privacy note is understated.** Not a big green badge like ClearCut — this tool's audience is broader (not just privacy-conscious). A small line under the subtitle: "🔒 Zero server uploads — fully private" in green text, no background.

### Specific Anti-Patterns to Avoid

| ❌ Do NOT | ✅ Instead |
|-----------|-----------|
| Dark mode | Light mode — visual tool |
| Multiple file upload in v1 | Single file only |
| Showing the compressed image preview | Show numbers (before/after size + %) — faster, clearer |
| Slider without visible percentage | Always show quality % next to slider |
| "Download" as a text link | "Download" as a full styled button |
| Technical jargon ("lossy", "quantization") | Simple language ("quality", "file size") |
| Auto-compress on upload | Let user configure, then click "Compress" |
| Compress button that's not full-width | Full-width primary button — unmissable |

### Mobile Design

Single column. Drop zone fills width. Quality slider has a large touch target (44px+ thumb). Target size presets become a horizontal scroll row. "Compress" button is full-width. Result card shows stats stacked vertically (original on top, arrow down, compressed below). Download button is sticky at bottom.

### Brand Assets Needed (Day 1)

| Asset | Spec | Notes |
|-------|------|-------|
| Favicon | ◆ diamond in blue square, 32×32 | Simple geometric mark |
| OG Image | 1200×630, white bg, "2.4 MB → 340 KB (−86%)" in big text | Communicates value instantly when shared on social |
| Logo mark | ◆ in blue rounded-square | CSS-only |

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                    BROWSER                           │
│                                                       │
│  1. User drops/selects image file                    │
│                                                       │
│  2. FileReader loads image into memory               │
│     (never leaves browser)                           │
│                                                       │
│  3. Image drawn to hidden <canvas>                   │
│                                                       │
│  4. Canvas re-exports at lower quality               │
│     canvas.toBlob(cb, 'image/jpeg', 0.7)            │
│                                                       │
│  5. If target size specified:                        │
│     Binary search on quality parameter               │
│     until output ≤ target size                       │
│                                                       │
│  6. Display before/after comparison                  │
│     - Original size vs compressed size               │
│     - Percentage reduction                           │
│     - Visual quality comparison                      │
│                                                       │
│  7. User clicks Download                             │
│     - Blob → Object URL → <a download>              │
│                                                       │
│  No server. No upload. No API. Zero cost.            │
└─────────────────────────────────────────────────────┘
```

---

## Project Structure

```
squish/
├── index.html                  # Main compressor tool
├── css/
│   └── style.css               # Clean, minimal styles
├── js/
│   ├── compressor.js           # Core: Canvas-based compression
│   ├── target-size.js          # Binary search for exact target size
│   ├── file-handler.js         # Drag-drop + file input handling
│   ├── ui.js                   # DOM updates, progress, animations
│   └── download.js             # Blob download helper
├── pages/                      # Size-specific landing pages
│   ├── compress-to-100kb.html
│   ├── compress-to-200kb.html
│   ├── compress-to-1mb.html
│   ├── compress-to-50kb.html
│   └── compress-to-20kb.html
├── blog/
│   ├── compress-image-without-losing-quality.html
│   ├── best-image-compressor-2026.html
│   ├── compress-image-for-email.html
│   ├── tinypng-alternative-free.html
│   └── jpg-vs-png-which-to-compress.html
├── favicon.ico
├── og-image.png
├── robots.txt
├── sitemap.xml
└── README.md
```

---

## Core Implementation

### Image Compression (Canvas API)

```javascript
// js/compressor.js

/**
 * Compress an image file using Canvas API
 * @param {File} file - Input image file
 * @param {number} quality - Quality 0.0 to 1.0 (JPG/WebP only)
 * @param {string} outputFormat - 'image/jpeg', 'image/png', or 'image/webp'
 * @returns {Promise<{blob: Blob, width: number, height: number}>}
 */
async function compressImage(file, quality = 0.7, outputFormat = 'image/jpeg') {
  return new Promise((resolve, reject) => {
    const img = new Image();
    const url = URL.createObjectURL(file);

    img.onload = () => {
      const canvas = document.createElement('canvas');
      canvas.width = img.naturalWidth;
      canvas.height = img.naturalHeight;

      const ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, 0);

      canvas.toBlob(
        (blob) => {
          URL.revokeObjectURL(url);
          if (blob) {
            resolve({
              blob,
              width: img.naturalWidth,
              height: img.naturalHeight,
            });
          } else {
            reject(new Error('Compression failed'));
          }
        },
        outputFormat,
        quality
      );
    };

    img.onerror = () => {
      URL.revokeObjectURL(url);
      reject(new Error('Failed to load image'));
    };

    img.src = url;
  });
}
```

### Target Size Compression (Binary Search)

```javascript
// js/target-size.js

/**
 * Compress image to a specific target file size
 * Uses binary search on quality parameter
 * 
 * @param {File} file - Input image
 * @param {number} targetBytes - Target size in bytes
 * @param {number} tolerance - Acceptable overshoot in bytes (default 5KB)
 * @returns {Promise<{blob: Blob, quality: number, attempts: number}>}
 */
async function compressToSize(file, targetBytes, tolerance = 5120) {
  let low = 0.01;
  let high = 1.0;
  let bestBlob = null;
  let bestQuality = 0;
  let attempts = 0;
  const maxAttempts = 15; // Binary search converges fast

  // First check: is the original already small enough?
  if (file.size <= targetBytes) {
    return { 
      blob: file, 
      quality: 1.0, 
      attempts: 0,
      alreadySmall: true 
    };
  }

  while (attempts < maxAttempts && (high - low) > 0.01) {
    const mid = (low + high) / 2;
    const { blob } = await compressImage(file, mid, 'image/jpeg');
    attempts++;

    if (blob.size <= targetBytes) {
      bestBlob = blob;
      bestQuality = mid;
      low = mid; // Try higher quality
    } else {
      high = mid; // Need lower quality
    }

    // Close enough? Stop early.
    if (blob.size <= targetBytes && blob.size >= targetBytes - tolerance) {
      break;
    }
  }

  // If even quality=0.01 is too big, return best effort
  if (!bestBlob) {
    const { blob } = await compressImage(file, 0.01, 'image/jpeg');
    return { blob, quality: 0.01, attempts, reachedMinimum: true };
  }

  return { blob: bestBlob, quality: bestQuality, attempts };
}
```

### Drag-and-Drop Handler

```javascript
// js/file-handler.js

function setupDropZone(dropZoneEl, onFile) {
  // Drag events
  ['dragenter', 'dragover'].forEach(event => {
    dropZoneEl.addEventListener(event, (e) => {
      e.preventDefault();
      dropZoneEl.classList.add('drag-over');
    });
  });

  ['dragleave', 'drop'].forEach(event => {
    dropZoneEl.addEventListener(event, (e) => {
      e.preventDefault();
      dropZoneEl.classList.remove('drag-over');
    });
  });

  dropZoneEl.addEventListener('drop', (e) => {
    const file = e.dataTransfer.files[0];
    if (file && file.type.startsWith('image/')) {
      onFile(file);
    }
  });

  // Click to select
  const fileInput = document.createElement('input');
  fileInput.type = 'file';
  fileInput.accept = 'image/jpeg,image/png,image/webp';
  fileInput.addEventListener('change', (e) => {
    if (e.target.files[0]) onFile(e.target.files[0]);
  });

  dropZoneEl.addEventListener('click', () => fileInput.click());
}
```

---

## SEO Page Structure (Main Page)

```
┌──────────────────────────────────────────┐
│  H1: "Compress Images Online — Free"     │
│  "Reduce file size without losing        │
│   quality. Your files never leave        │
│   your browser."                         │
├──────────────────────────────────────────┤
│                                          │
│  ┌──────────────────────────────────┐    │
│  │                                  │    │
│  │     [DROP IMAGE HERE]            │    │  ← Above the fold
│  │     or click to upload           │    │
│  │                                  │    │
│  │     JPG · PNG · WebP             │    │
│  │     Max 20MB                     │    │
│  │                                  │    │
│  └──────────────────────────────────┘    │
│                                          │
│  Quality slider: ──●──────── 70%         │
│  OR                                      │
│  Target size: [ 100 ] KB                 │
│                                          │
│  [Compress] button                       │
│                                          │
├──────────────────────────────────────────┤
│  RESULT (after compression):             │
│                                          │
│  ┌────────────┐    ┌────────────┐        │
│  │  Original  │    │ Compressed │        │
│  │  2.4 MB    │ →  │  340 KB    │        │
│  │            │    │  -86%      │        │
│  └────────────┘    └────────────┘        │
│                                          │
│  [Download Compressed Image]             │
│  [Compress Another]                      │
├──────────────────────────────────────────┤
│  Quick links:                            │
│  Compress to 100KB | 200KB | 1MB | 50KB  │  ← Internal links
├──────────────────────────────────────────┤
│  [Ad: one banner]                        │
├──────────────────────────────────────────┤
│  H2: "How to Compress Images Online"     │
│  H2: "JPG vs PNG Compression"            │
│  H2: "FAQ"                              │
└──────────────────────────────────────────┘
```

---

## Size-Specific Landing Pages

Each page is the same tool but with the target size pre-filled. This captures the extremely high-intent "compress image to X" searches.

```html
<!-- pages/compress-to-100kb.html -->
<h1>Compress Image to 100KB — Free Online Tool</h1>
<p>Reduce any image to under 100KB instantly. 
   No upload, no sign up. Runs in your browser.</p>

<!-- Same tool component, but with targetSize=102400 pre-set -->
<script>
  // Pre-fill the target size input
  document.getElementById('target-size').value = 100;
</script>

<!-- SEO content specific to this size -->
<h2>Why Compress Images to 100KB?</h2>
<p>Many online forms, job applications, and government portals 
   require images under 100KB. This tool automatically finds 
   the optimal quality setting to hit exactly 100KB...</p>
```

---

## 7-Day Distribution Plan

### Day 1–2: Build & Deploy
- Build main compressor page + 3 size-specific pages (100KB, 200KB, 1MB).
- Deploy to Vercel. Custom domain.
- Submit to Google Search Console.

### Day 3: Privacy-Angle Content
Post in communities where privacy matters:
- r/privacy: "Built an image compressor that runs 100% in your browser — zero server uploads"
- r/webdev: "Client-side image compression using Canvas API — no backend needed"
- Hacker News: "Show HN: Browser-based image compressor (your files never leave your device)"

### Day 4: Use-Case Communities
- r/Etsy: "Free tool to compress product photos for faster page loads"
- r/resumes: "Compress your headshot for job applications (many portals have size limits)"
- Photography forums: "Quick client-side image compression tool"
- Web development Discord servers

### Day 5–6: SEO Content
Write and publish:
1. "How to Compress an Image to Under 100KB (3 Methods)"
2. "TinyPNG vs [Squish] — Which Is Truly Free?"
3. "How to Compress Images Without Losing Quality"
4. "Best Image Compressor 2026 (Tested & Ranked)"

### Day 7: Expand
- Build /compress-to-50kb and /compress-to-20kb pages
- Add internal links between all pages
- Add PNG-specific compression page targeting `compress png online`

---

## Performance Targets

| Metric | Target |
|--------|--------|
| Page weight | < 80KB (no framework) |
| Time to compress 5MB JPG | < 2 seconds |
| Lighthouse score | 98+ |
| CLS | 0 |

The tool must feel instant. Image compression via Canvas API takes 100–500ms for typical photos. The user experience is: drop image → see result in under 1 second.

---

## Environment Variables

```bash
# None. Zero. This is a static site with no backend.
# AdSense code and Plausible script are added directly to HTML.
```

---

## Launch Commands

```bash
# Option 1: Pure static (recommended for v1)
# Just deploy the folder
vercel --prod

# Option 2: Vite for dev server + minification
npm create vite@latest squish -- --template vanilla
cd squish
npm run dev
npm run build
vercel --prod
```

---

## Expansion Roadmap (NOT v1)

- [ ] PNG compression via Squoosh WASM codecs
- [ ] WebP output option (smaller than JPG)
- [ ] Batch compression (drag multiple files)
- [ ] Resize + compress combo
- [ ] Browser extension for right-click compress
- [ ] PWA for offline use

---

## License

MIT License
