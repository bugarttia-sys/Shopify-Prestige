# Image Upload Guidelines — Grand Master Spas Product Pages

> **Theme:** Prestige by Maestrooo  
> **Last updated:** March 2026  
> **Purpose:** Ensure every image on the product page is sharp on all devices, including retina/HiDPI displays.

---

## Why Image Size Matters

Shopify's CDN automatically converts images to WebP/AVIF and serves the best format for each browser. However, **it cannot upscale images**. If you upload a 1200px-wide image and the theme requests a 2400px version for a retina display, Shopify will serve the 1200px original — resulting in a blurry image.

The Prestige theme generates `srcset` attributes with widths up to **3200px** for full-width sections. To take full advantage of this, source images must be uploaded at sufficient resolution.

---

## Minimum Upload Sizes by Section

| Page location | Section type | Minimum upload size | Aspect ratio | Format |
|---|---|---|---|---|
| **Hero banner** | `custom-image-with-text-overlay` | **3200 × 1800 px** | 16:9 | JPG (quality 80–85) or WebP |
| **Hero banner (mobile)** | Mobile image setting | **1000 × 1400 px** | ~5:7 portrait | JPG or WebP |
| **Product gallery** | `main-product` → `media.liquid` | **2048 × 2048 px** | 1:1 square | JPG or PNG (for transparent bg) |
| **Design story / Interior story** | `image-with-text-block` | **3200 × 2400 px** | 4:3 | JPG (quality 80–85) |
| **Feature sections** (Hydrotherapy, Energy, Water Care, Entertainment) | `image-with-text` | **3200 × 2400 px** | 4:3 | JPG (quality 80–85) |
| **Exterior gallery grid** | `media-grid` | **1600 × 1200 px** per tile | Varies by tile | JPG (quality 80–85) |
| **Scroll story images** | `images-with-text-scroll` | **1725 × 1725 px** | ~1:1 | JPG (quality 80–85) |
| **Comparison CTA banner** | `image-with-text-overlay` | **3200 × 1800 px** | 16:9 | JPG (quality 80–85) |
| **Product cards** (related/recently viewed) | `product-card` | **2048 × 2048 px** | 1:1 square | JPG or PNG |
| **Icons** (text-with-icons section) | `text-with-icons` | **2× display size** | Original | SVG preferred, PNG fallback |
| **Logo** | Header | **2× display size** | Original | SVG preferred, PNG fallback |

---

## Current Known Placeholder Issues

These sections currently reuse the same image as a placeholder. Each should have a **unique, purpose-specific image** uploaded:

### Problem: Same product image used 4× as placeholder

The sections `hydrotherapy_detail`, `energy_detail`, `water_care_detail`, and `entertainment_detail` in `product.spa-premium.json` all reference `{{ product.featured_image }}`. Each needs a unique image:

| Section | Recommended image content |
|---|---|
| Hydrotherapy | Close-up of jets/massage seats in action |
| Energy Management | Insulation system or cover detail |
| Water Care | Filtration system or crystal-clear water |
| Entertainment | LED lighting, audio, or ambiance shot |

**How to fix:** In the Shopify admin, go to **Online Store → Customize → Products → [product]**, select each section, and upload a unique image via the Image picker setting.

### Problem: Same design story image used 3×

The sections `design_story_1`, `interior_story`, and `technology_story` all reference `{{ product.metafields.media.design_story.value }}`. To fix, create separate media metafields:

- `product.metafields.media.design_story` — exterior design/curves
- `product.metafields.media.interior_story` — interior seats/ergonomics
- `product.metafields.media.technology_story` — technology/engineering detail

Then update each section's image setting in the template to reference the correct metafield.

---

## Image Optimization Checklist

### Before uploading

- [ ] Image meets the minimum size for its section (see table above)
- [ ] Image is saved as JPG at quality 80–85 (balances quality vs. file size)
- [ ] Image uses sRGB color profile (not Adobe RGB or CMYK)
- [ ] Image filename is descriptive (e.g., `ecstatic-hydrotherapy-jets-detail.jpg`, not `Image1.jpg`)
- [ ] No embedded metadata/EXIF beyond essentials (reduces file size)

### Do NOT do

- **Do not upscale** small images — this just makes blurry images bigger. Reshoot or source higher-resolution originals.
- **Do not add text overlays** to images — text should be in the Liquid template so it's translatable and responsive.
- **Do not use PNG** for photographs — JPG or WebP are much smaller. Reserve PNG for images requiring transparency.
- **Do not apply `image-rendering: crisp-edges`** CSS to photographs — this is for pixel art/icons and makes photos look worse.

---

## How Prestige Renders Images

For reference, the Prestige theme uses Shopify's modern `image_url` + `image_tag` pipeline:

1. `image_url: width: image.width` — requests the original resolution from Shopify CDN
2. `image_tag` — generates an `<img>` with:
   - `srcset` — multiple widths so the browser picks the best size
   - `sizes` — tells the browser the display width to use for selection
   - `width` and `height` — prevents Cumulative Layout Shift (CLS)
   - `loading="lazy"` — defers off-screen images (except hero/first gallery image)
   - `fetchpriority="high"` — prioritizes above-the-fold images

Shopify's CDN automatically:
- Serves WebP or AVIF when the browser supports it
- Resizes on the fly based on the requested `width` parameter
- Caches aggressively at the edge

**You do not need any third-party image optimization apps.** Just upload high-quality source images and the pipeline handles everything.

---

## Quick Reference: Retina Math

| Display width | 1× (standard) | 2× (retina) | 3× (mobile retina) |
|---|---|---|---|
| 500px | 500px | 1000px | 1500px |
| 800px | 800px | 1600px | 2400px |
| 1200px | 1200px | 2400px | 3600px |
| 1600px (full-width desktop) | 1600px | 3200px | — |

This is why a 1600×1200px source image looks blurry on retina: at 2× it renders at effectively 800×600px. Upload at minimum **2× your expected display width**.
