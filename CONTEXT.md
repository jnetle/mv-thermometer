# MV Thermometer — Context

_Last updated: 2026-06-02_

Interactive fundraising thermometer widget for Mira Vista School's annual campaign. Built from the school's original SVG design file. 40 honeycomb cells fill with yellow (bottom→top) animated from a hard-coded percentage derived from dollar amounts. Ends with confetti + a shake-then-single-pulse on the dollar amount.

---

## Goals / targets
- Embed on miravistaschool.com (Squarespace) as a live progress tracker
- Staff update progress by editing two JS constants and re-uploading — no coding knowledge needed
- Preserve the original poster design as faithfully as possible

---

## Tools / stack
- **Source design:** `Mira Vista Annual Fundraising.svg` (original, 1.5 MB — kept for reference only)
- **Python / Pillow / cairosvg:** SVG→PNG rendering, surgical pixel editing of the background
- **HTML/CSS/JS:** single-file widget, base64-embedded background PNG
- **Netlify Drop:** free hosting at a stable URL → Squarespace iframe embed

---

## Source image pipeline
Starting point for all PNG edits is **`bg_fixed.png`** (stored in Claude's session outputs, not in the project folder). It is the original design rendered at 800px wide with the logo's black background box removed (black pixels replaced with poster blue).

Every widget rebuild applies these transformations to `bg_fixed.png` in order:
1. **Logo fix** — circle-based mask: pixels outside radius=31 from center (154,79) painted blue; interior left untouched to preserve bee, text, white circle
2. **GOAL section cleared** — y=195–481, x=378–736 painted blue (removes "GOAL" label + white pill + 100% label area)
3. **100% label restored** — non-blue original pixels copied back at y=188–258, x=498–702 (yellow fill + dark outline)
4. **Top-right dashed arc restored** — dark pixels (max channel < 140) copied from original at y=128–285, x=530–806
5. **PayPal/Venmo removed** — surgical per-element approach:
   - Transition zone y=905–935, x=407–750: keep only bee-yellow pixels, paint everything else blue
   - Solid paint y=935–1118, x=407–752 (stops before website URL at y=1122)
6. Output saved and base64-encoded into the HTML

---

## Widget structure
```
<div class="widget-container">
  <div class="thermometer-wrap">        ← position:relative container
    <canvas id="confetti-canvas"/>       ← z-index:10, confetti drawn here
    <img class="bg-image"/>              ← base64 background PNG
    <svg class="honeycomb-overlay"/>     ← 40 hexagon polygons, viewBox 0 0 2664 4032
  </div>
  <div class="info-panel">              ← below image, not overlaid
    raised-label (reads from SCHOOL_YEAR)
    raised-amount (yellow, animates post-confetti)
    of-goal
  </div>
</div>
```

---

## Honeycomb details
- **40 cells** total, ordered bottom→top (index 0 = bottom-left, 39 = top-right)
- Coordinates extracted from SVG clipPath elements (6-vertex polygons) in the original 2664×4032 SVG coordinate space
- Fill sequence: `Math.round((CURRENT_AMOUNT / GOAL_AMOUNT) * 40)` cells fill
- Animation: 110ms per cell, flash→filled transition, confetti fires after last cell

## Animation sequence
1. 600ms delay on page load → cells fill bottom-to-top (110ms each)
2. Last cell fills → confetti launches after 250ms
3. 900ms after last fill → dollar amount shakes once (0.55s), then pulses once (2s golden glow)

---

## Squarespace deployment
**Recommended:** Netlify Drop
1. Go to app.netlify.com/drop
2. Drag `fundraising_thermometer.html` into the browser
3. Copy the generated URL
4. In Squarespace: add a **Code** block, paste:
   ```html
   <iframe src="YOUR_NETLIFY_URL" width="100%" height="950" frameborder="0" scrolling="no" style="display:block;"></iframe>
   ```
5. To update: drag the new HTML file to Netlify — same URL is preserved

**Alternative:** Paste HTML directly into a Squarespace Code block (requires Business plan or above).

---

## Conventions
- Source for all rebuilds: `bg_fixed.png` — never start from an intermediate
- Honeycomb border: `stroke-width:10`, `fill:white` on empty cells (covers original PNG borders), no `paint-order` override
- Font: `'Arial Black', Arial, sans-serif` throughout
- Colors: poster blue `#1069cb`, honeycomb yellow `#ffce00`, accent yellow `#fab918`
- School logo circle: center≈(154,79) in 800px PNG, radius≈31px

---

## Status
**Complete and production-ready as of 2026-06-02.**

Outstanding / future:
- If the school year changes, update `SCHOOL_YEAR` constant
- If goal changes, update `GOAL_AMOUNT`
- If design needs refreshing, rerun the image pipeline from `bg_fixed.png` with updated parameters

---

## Key files
| File | Location | Notes |
|---|---|---|
| `fundraising_thermometer.html` | MV Thermometer folder | The deliverable — edit constants to update |
| `CLAUDE.md` | MV Thermometer folder | Quick-start reference |
| `CONTEXT.md` | MV Thermometer folder | This file |
| `Mira Vista Annual Fundraising.svg` | Uploaded by Jeanette | Original source design (1.5 MB) |
| `bg_fixed.png` | Claude session outputs | Logo-fixed source PNG — regenerate if session is lost by re-rendering the SVG with cairosvg at 800px wide and reapplying the black-pixel-to-blue fix on y=48–110, x=100–206 |
