# 🐝 Mira Vista Fundraising Thermometer

A self-contained fundraising progress widget for Mira Vista School's annual campaign. Forty honeycomb cells fill with yellow from the bottom up based on how close the campaign is to its goal, then confetti bursts and the dollar amount celebrates. It's responsive, has no external dependencies, and the numbers are set through the link, so a non-technical person can keep it up to date.

## Live

- **Widget** (this is what gets embedded): https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html
- **Builder** (no-code control panel): https://jnetle.github.io/mv-thermometer/thermometer_builder.html

## Update the numbers (no coding, no re-uploading)

The widget reads its values from the link itself:

```
…/fundraising_thermometer.html?year=2025-26&goal=150000&current=32500
```

So to update progress:

1. Open the **builder**, type the new School Year / Goal / Raised amounts (live preview updates as you type).
2. Click **Copy embed code**.
3. On your website (Wix or Squarespace), open the embed/HTML block and replace the old embed code with the new block.

That's it. Full step-by-step, plan requirements, and an "advanced" path for changing the built-in defaults are in **[HOSTING.md](./HOSTING.md)**.

## What's in here

| File | What it is |
|------|------------|
| `fundraising_thermometer.html` | The widget. Self-contained (~235 KB, background image embedded as base64). Reads `?year=&goal=&current=` from the URL, falling back to baked-in defaults. |
| `thermometer_builder.html` | A browser UI to set the numbers, preview, and copy a ready-to-paste embed snippet. Holds a tokenized snapshot of the widget. |
| `HOSTING.md` | Hosting and update instructions for the maintainer. |
| `CONTEXT.md` | Fuller reference: design pipeline, structure, conventions. |
| `CLAUDE.md` | Quick-start notes for working on the project. |

## How it stays responsive

- **Fits any width with no scrollbars** — the widget reports its rendered height to the embedding page via `postMessage`, and the embed snippet auto-sizes the iframe to match.
- **Text scales with the widget** — the info panel uses CSS container queries (`cqw`), so the amount, label, and goal shrink smoothly on narrow screens.

## Tech

Plain HTML/CSS/JS, no build step or dependencies. Hosted free on GitHub Pages. The background art was produced from the school's original SVG via Python + Pillow + cairosvg (see CONTEXT.md).
