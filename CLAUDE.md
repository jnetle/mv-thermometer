# CLAUDE.md — MV Thermometer

A self-contained HTML fundraising thermometer widget for Mira Vista School's annual campaign. Honeycombs fill with yellow from bottom to top, then confetti bursts and the dollar amount shakes + pulses once.

**Full context lives in [`CONTEXT.md`](./CONTEXT.md)** — read it for workflow, image pipeline, and Squarespace deployment details.

## The one file that matters
`fundraising_thermometer.html` — single self-contained file (~228 KB). The background image is embedded as base64 PNG. Open in any browser to preview.

## Update fundraising progress
Edit these three constants near the bottom of the `<script>` tag:
```js
const SCHOOL_YEAR    = "2025-26";
const GOAL_AMOUNT    = 150000;
const CURRENT_AMOUNT = 32500;
```
Save and re-upload to Netlify. No other changes needed.

## Conventions
- Never re-use old base64 blobs — always regenerate from `bg_fixed.png` source
- Honeycomb overlay SVG uses `viewBox="0 0 2664 4032"` matching the original SVG coordinate space
- Empty honeycombs: `fill: white` (not transparent) — prevents double-border artifact from the background PNG
- Stroke on honeycombs: `stroke-width: 10`, default `fill stroke` paint order for consistent border weight
- All PNG edits start from `bg_fixed.png` (logo-fixed source); never from intermediate outputs

## Status
Widget is complete and production-ready. Squarespace deployment via Netlify Drop iframe is the recommended path.

## Tools
Python 3 + Pillow (image surgery) · cairosvg (SVG→PNG) · HTML/CSS/JS (widget) · Netlify Drop (hosting)
