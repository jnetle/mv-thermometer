# CLAUDE.md — MV Thermometer

A self-contained HTML fundraising thermometer widget for Mira Vista School's annual campaign. Honeycombs fill with yellow from bottom to top, then confetti bursts and the dollar amount shakes + pulses once.

**Full context lives in [`CONTEXT.md`](./CONTEXT.md)** — read it for the image pipeline, structure, and conventions. **[`README.md`](./README.md)** is the public landing page; **[`HOSTING.md`](./HOSTING.md)** is the maintainer's update guide.

## The files that matter
- `fundraising_thermometer.html` — the self-contained widget (~235 KB; background image is base64 PNG). Open in any browser to preview.
- `thermometer_builder.html` — a no-code UI to set the numbers, preview, and copy an embed snippet. Embeds a tokenized snapshot of the widget in a hidden `<textarea id="tpl">`.
- `HOSTING.md` — hosting + update instructions for the non-technical maintainer.

Hosted on GitHub Pages, repo `jnetle/mv-thermometer` (public):
- Widget (embed this): https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html
- Builder (control panel): https://jnetle.github.io/mv-thermometer/thermometer_builder.html

## Update fundraising progress
**Preferred (no re-upload):** the widget reads its values from URL query params, so updating is just changing the embed link. Open the builder, set the numbers, **Copy embed code**, and replace the `<iframe>` line on the website. The link looks like:
```
…/fundraising_thermometer.html?year=2025-26&goal=150000&current=32500
```
Missing/blank/non-numeric params fall back to the baked-in defaults below.

**To change the baked-in defaults** (shown when opened with no params), edit these three constants near the top of the `<script>` tag and re-upload to GitHub Pages:
```js
const SCHOOL_YEAR    = _params.get('year') || "2025-26";
const GOAL_AMOUNT    = _num('goal', 150000);
const CURRENT_AMOUNT = _num('current', 32500);
```

> If you change the widget itself, regenerate the builder's `<textarea id="tpl">` snapshot (read widget → tokenize the 3 default literals → HTML-escape `& < >` → splice into the textarea).

## Responsive behavior (don't break these)
- **Auto-height:** the widget `postMessage`s its rendered height to the parent; the builder preview and the embed snippet size the iframe to match (responsive, no scrollbars). Reporter lives at the end of the widget `<script>`.
- **Container-query text:** `.info-panel` is `container-type: inline-size`; the label / amount / of-goal font sizes use `cqw` with low `clamp()` floors so they scale all the way down on narrow widths.

## Conventions
- Never re-use old base64 blobs — always regenerate from `bg_fixed.png` source
- Honeycomb overlay SVG uses `viewBox="0 0 2664 4032"` matching the original SVG coordinate space
- Empty honeycombs: `fill: white` (not transparent) — prevents double-border artifact from the background PNG
- Stroke on honeycombs: `stroke-width: 10`, default `fill stroke` paint order for consistent border weight
- All PNG edits start from `bg_fixed.png` (logo-fixed source); never from intermediate outputs

## Status
Complete and production-ready. Hosted on GitHub Pages; embedded via an iframe whose link carries the numbers as query params. See `HOSTING.md`.

## Tools
Python 3 + Pillow (image surgery) · cairosvg (SVG→PNG) · HTML/CSS/JS (widget + builder) · GitHub Pages (hosting)
