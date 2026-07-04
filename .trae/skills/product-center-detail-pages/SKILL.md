---
name: "product-center-detail-pages"
description: "Builds product-center category cards, product list cards, and matching detail pages. Invoke when user uploads product images for Product Center or asks to add product details."
---

# Product Center Detail Pages

## Purpose

Use this skill when the user wants to add or update Product Center categories, category list pages, product cards, and corresponding product detail pages for the Handan Dingrui Metal Products website.

The expected website pattern is:

- Homepage Product Center card shows the category main product image.
- Category page shows a grid of product cards.
- Each product card uses a real product image, an English product name, a short English material/spec line, and a red `VIEW DETAILS` button.
- Each card links to a dedicated product detail page.
- Each detail page shows the matching product image, overview, specifications, application scenarios, and inquiry CTA.

## Invocation Triggers

Invoke this skill when the user:

- Uploads product images and says to add them to 产品中心 / Product Center.
- Says a product belongs to the first, second, third, etc. Product Center card.
- Says one image is the category main image and the rest are product detail images.
- Asks to make product detail pages like a previous category.
- Says “像这个一样”, “像先前一样”, “生成对应详情页”, “添加到产品中心卡片”, or similar.

## Visual Standard

Product category list pages must follow this card style:

- Page background: light gray, close to `#f1f4f8`.
- Card background: pure white.
- Card shape: large rounded corners, around `24px`.
- Card border: no visible border.
- Card shadow: soft, subtle, industrial-clean.
- Card image area: large white image stage, fixed height around `260px` to `270px`.
- Product image: `object-fit: contain`, never cropped.
- Text area: English product name, bold navy, around `22px`.
- Description/material line: muted blue-gray text.
- Button: red solid pill button, white text, label `VIEW DETAILS`.
- Grid: desktop uses 3 columns, tablet 2 columns, mobile 1 column.

Recommended card CSS:

```css
body{background:#f1f4f8}
.product-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:28px}
.product-card{background:#fff;border-radius:24px;overflow:hidden;transition:all .35s cubic-bezier(.4,0,.2,1);cursor:pointer;display:flex;flex-direction:column;box-shadow:0 4px 20px rgba(11,35,65,.06)}
.product-card:hover{transform:translateY(-6px);box-shadow:0 20px 50px rgba(11,35,65,.14)}
.product-card:hover .card-img img{transform:scale(1.05)}
.card-img{height:270px;overflow:hidden;background:#fff;display:flex;align-items:center;justify-content:center;padding:20px}
.card-img img{width:100%;height:100%;object-fit:contain;transition:transform .5s cubic-bezier(.4,0,.2,1)}
.card-info{padding:22px 24px 28px;flex:1;display:flex;flex-direction:column}
.card-info h3{font-size:22px;color:#0b2341;margin-bottom:8px;font-weight:900;line-height:1.25}
.card-info p{font-size:14px;color:#687386;margin-bottom:20px;flex:1;line-height:1.6}
.card-btn{display:inline-flex;align-items:center;justify-content:center;height:40px;padding:0 24px;border-radius:999px;background:#e60012;color:#fff;font-weight:800;font-size:13px;align-self:flex-start;box-shadow:0 6px 16px rgba(230,0,18,.25)}
@media(max-width:900px){.product-grid{grid-template-columns:repeat(2,1fr)}}
@media(max-width:600px){.product-grid{grid-template-columns:1fr}}
```

## Workflow

1. Identify the target Product Center category.
   - If the user says “第一个卡片”, map to Bolt Class.
   - If the user says “第二个卡片”, map to Nuts.
   - If the user says “第三个卡片”, map to Studs.
   - If the user says another card, inspect `index.html` to identify the target category and existing link.

2. Save uploaded images into `assets/products/`.
   - Use English, lowercase, hyphenated filenames.
   - Never keep Chinese filenames inside the final website asset references.
   - Preserve the original extension unless conversion is needed.
   - If the first uploaded image is stated as the main image, use it for the homepage category card.
   - If the user is adding more products to an existing category, merge the new product cards into that category page instead of reverting to an old overview layout.
   - If an earlier category main image is a placeholder, broken image, or unsuitable product visual, replace it with the most representative newly uploaded product image.

3. Update homepage Product Center category card in `index.html`.
   - Replace `.category-ghost` with:

```html
<div class="category-visual">
  <img class="category-photo" src="assets/products/<main-image>" alt="<Product Category>" />
</div>
```

   - Keep the card link pointing to the category list page.
   - Keep hover actions: `查看详情 →` and `快速询价`.
   - Quick RFQ should go to `index.html#contact` or `#contact` depending on current page context.

4. Create or rewrite the category list page.
   - Examples:
     - Bolts: `products/bolt-class.html`
     - Nuts: `products/nuts-class.html`
     - Studs: `products/double-headed-studs.html`
   - The category list page is not a single detail page.
   - It must show product cards, each linking to its own detail page.
   - When a category already has product cards, preserve existing valid cards unless the user explicitly asks to replace them.
   - If regenerating the category page is easier, reconstruct the complete current product list from existing product data and new uploads so no valid product disappears.

5. Create detail pages under a category folder.
   - Examples:
     - `products/bolts/<slug>.html`
     - `products/nuts/<slug>.html`
     - `products/studs/<slug>.html`
   - Every uploaded product image should have a corresponding detail page unless the user explicitly says it is only a supporting image.

6. Detail page content must be English-only unless the user explicitly asks otherwise.
   - Do not mix Chinese and English in product detail page content.
   - Recommended sections:
     - Hero with product name and main image.
     - Product Overview.
     - Key Specifications.
     - Application Scenarios.
     - Quick RFQ CTA.

7. Verify locally.
   - Check the category page link from homepage.
   - Check at least one product card link.
   - Check image paths and page loading.

## Detail Page Standard

Each detail page should include:

- `h1`: product name.
- Hero product image using the same product image as its list card.
- `Product Overview`: one paragraph explaining what it is and where it is used.
- `Key Specifications` cards:
  - Material
  - Standard
  - Grade
  - Surface
- `Application Scenarios`: 5 short scenario cards.
- CTA:
  - Heading: `Need a quotation?`
  - Button: `Quick RFQ`
  - Link: `../../index.html#contact`

Recommended detail image behavior:

```css
.product-stage{position:relative;min-height:560px;border-radius:40px;background:linear-gradient(145deg,rgba(255,255,255,.96),rgba(238,243,250,.9));box-shadow:0 32px 80px rgba(0,0,0,.24);display:grid;place-items:center;overflow:hidden}
.product-stage img{position:relative;z-index:2;width:min(94%,680px);max-height:500px;object-fit:contain;filter:drop-shadow(0 30px 28px rgba(11,35,65,.25));transform:rotate(-4deg)}
.detail-photo{margin-top:24px;border-radius:26px;background:#fff;box-shadow:0 18px 48px rgba(11,35,65,.08);padding:28px;display:grid;place-items:center}
.detail-photo img{width:min(100%,760px);max-height:520px;object-fit:contain;filter:drop-shadow(0 24px 24px rgba(11,35,65,.16))}
```

## Product Data Guidance

When generating product names and details:

- Translate Chinese filenames into clean English product names.
- Use industrial fastener terminology.
- Avoid fabricated certifications.
- It is acceptable to infer common material, grade, standard, surface, and application ranges from product type, but keep claims generic.
- Examples:
  - 六角螺母 → `Hex Nut`
  - 不锈钢六角螺母 → `Stainless Steel Hex Nut`
  - 双头螺柱 → `Double End Stud`
  - 不锈钢全螺纹螺柱 → `Stainless Steel Full Thread Stud`
  - 特氟龙全螺纹螺柱 → `PTFE Coated Full Thread Stud`
  - 不锈钢平垫圈 → `Stainless Steel Flat Washer`
  - 不锈钢弹垫圈 → `Stainless Steel Spring Washer`
  - 弹垫圈 → `Spring Washer`
  - 高强度平垫圈 → `High-Strength Flat Washer`
  - 高强度弹垫圈 → `High-Strength Spring Washer`

## Important Rules

- Do not make the category page itself act as a single product detail page.
- Do not put all uploaded images into one detail page unless the user explicitly asks for a gallery.
- Each product card should correspond to one detail page.
- Existing category products should not be lost when adding new uploaded images; update by merging, not by accidental overwrite.
- Keep final website files in the selected workspace folder.
- Temporary scripts should be created outside the final workspace, then executed to generate the final pages.
- Do not sync to GitHub unless the user explicitly asks.

## User Confirmation Pattern

After completing the work, reply in Chinese and mention:

- Which Product Center card was updated.
- Which category list page was generated/updated.
- Which detail pages were generated/updated.
- Local preview URLs.
- Whether GitHub was not synced.
