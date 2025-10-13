# Hard Hat Customizer

A single-page hard hat artwork proofing tool designed for Jendco Safety Supply. The page can be embedded inside BigCommerce's WYSIWYG editor (all layout/styling is inline) and lets the sales team place customer logos on top of vendor-provided helmet renders, clone artwork across views, and export a production-ready PDF proof.

## Table of Contents
- [Features](#features)
- [Getting Started](#getting-started)
- [Using the Customizer](#using-the-customizer)
- [Artwork & Print Area Guidance](#artwork--print-area-guidance)
- [Generating the Proof PDF](#generating-the-proof-pdf)
- [Managing Helmet Variants](#managing-helmet-variants)
- [Project Structure](#project-structure)

## Features
- **Four-sided canvas** powered by [Konva.js](https://konvajs.org/) with independent stages for the front, back, left, and right views, including drag/resize handles and min-size guarding.
- **Cascading vendor → hard hat → color dropdowns.** Vendor selection filters the available product variants, auto-fills the product code, and unlocks color choices for the selected base model.
- **Upload and edit customer artwork.** Drop PNG, JPG, JPEG, WEBP, or SVG files onto each side, constrain artwork to the defined print bounds, and view real-time DPI quality labels calculated against a 6"×6" maximum printable area.
- **Power tools for the sales team** such as cloning an uploaded logo to all sides, deleting the active selection, and capturing the last uploaded asset for quick reuse.
- **Inline styling** to survive WYSIWYG sanitization plus subtle button hover effects added at runtime.
- **One-click PDF proof export** generated with [PDF-Lib](https://pdf-lib.js.org/) that embeds base renders when available, includes customer/order metadata, and creates logo-only previews alongside the 2×2 helmet grid.

## Getting Started
This project is fully static—no build tooling is required.

1. Clone or download the repository.
2. Open `index.html` in any modern desktop browser (Chrome, Edge, Firefox, or Safari).
3. The app loads external CDNs for Konva and PDF-Lib; ensure you have an internet connection if you are opening the file locally.

> **Tip:** Because the markup relies entirely on inline styles, you can paste the contents of `index.html` into the BigCommerce WYSIWYG editor if you prefer to host it inside the storefront instead of running it locally.

## Using the Customizer
1. Choose a **Vendor**; selecting "Other" bypasses filtering and unlocks all variants.
2. Pick a **Hard Hat Variant**. The dropdown lists base models derived from the master data set; after you make a selection the **Color** list becomes available.
3. Select a **Color** (if the chosen model has multiple finishes). The stage backgrounds update immediately and the product code field auto-populates.
4. Upload customer artwork for each view by clicking **Upload to Front/Back/Left/Right**. Drag to reposition the logo, use the on-canvas handles to scale or rotate, and review the DPI quality badge to ensure the artwork meets print standards.
5. Use the **Clone selected → All** buttons to copy the latest selection across the remaining sides or **Delete Selected** to clear it.
6. Fill in **Order / Print Details** and **Customer Information** to include them in the final proof.

## Artwork & Print Area Guidance
- Each canvas highlights the printable region with a dashed rectangle; artwork snaps back inside these bounds whenever it is moved or transformed.
- DPI quality is rated Good (green), Fair (amber), or Poor (red) based on the uploaded image's native resolution versus the print area's size. Replace low-resolution logos before exporting the proof.
- The Konva stage maintains a maximum 6" width equivalent; unusually large uploads are downscaled to roughly 60% of the stage so users can see the full design immediately.

## Generating the Proof PDF
Click **Download Proof PDF** to build a landscape, letter-sized proof document that includes:
- Company branding header plus the selected helmet variant name.
- Customer/vendor/order metadata pulled from the form inputs.
- A 2×2 grid showing each helmet angle with optional base renders beneath the artwork overlay.
- A right-hand column of logo-only previews cropped from the latest artwork on every side.
- Footer copy for approvals, timestamps, and disclaimers.

If a vendor image fails to load (e.g., due to a CDN error) the process continues and the artwork overlay is still embedded so you always get a usable proof.

## Managing Helmet Variants
All helmet metadata lives in the `VARIANTS` object inside `index.html`. Each entry includes:

```js
{
  name: "V-Gard H2 Safety Helmet - Vented - White - 10242629",
  vendor: "MSA",
  productCode: "10242629",
  front: "https://.../Front.png",
  back: "https://.../Back.png",
  left: "https://.../Left.png",
  right: "https://.../Right.png"
}
```

- Add new products by appending objects keyed by a unique identifier (e.g., SKU).
- The app automatically derives the base model name and available colors using the `extractColor`/`removeColor` helpers, so maintain the `" - Color - "` naming convention for smooth filtering.
- If you have a vendor that should bypass filtering, set `vendor: "Other"` or choose "Other" in the dropdown while configuring a proof.

## Project Structure
```
.
├── index.html                     # Complete application markup, scripts, and styles
├── hard-hat-customizer-current.txt # Snapshot of the customizer markup for WYSIWYG editors
└── README.md                      # You are here
```

Feel free to tailor the styling, copy, or data set to match additional vendors as your catalog grows.
