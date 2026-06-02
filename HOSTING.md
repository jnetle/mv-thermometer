# Hosting & Updating the Thermometer

This project is hosted free on **GitHub Pages**. Two files live in the repo:

| File | What it's for | Lives at |
|------|---------------|----------|
| `fundraising_thermometer.html` | The widget visitors see. **This is the one you embed.** | `https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html` |
| `thermometer_builder.html` | Your private control panel for generating the widget. Not embedded anywhere. | `https://jnetle.github.io/mv-thermometer/thermometer_builder.html` |

> The repo is **public** (required for free GitHub Pages). Nothing sensitive is in it, so that's fine.

---

## How the numbers work: query parameters

The widget reads its values from the **link itself**, like this:

```
https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html?year=2025-26&goal=150000&current=32500
```

- `year` — the school year text (e.g. `2025-26`)
- `goal` — the goal dollar amount, digits only (e.g. `150000`)
- `current` — the amount raised so far, digits only (e.g. `32500`)

If a parameter is missing, the widget falls back to the defaults baked into the file. **This means you change the numbers by changing the link, not by re-uploading the file.**

---

## Embedding it on a website (Wix or Squarespace)

1. Open the **builder** (link above) and bookmark it.
2. Type your School Year / Goal / Raised numbers. The preview updates as you type.
3. Click **Copy embed code** — you get an `<iframe>` (plus a tiny auto-resize script) whose link already contains your numbers. It's responsive and sizes itself to fit, so there are no scrollbars. Copy the **whole** block.
4. In your site editor, add an **Embed / HTML block** and paste it.

**Plan notes:**
- **Squarespace** — needs the **Core plan ($23/mo)** or higher; the cheapest Basic plan won't run iframes. Use an *Embed Block*.
- **Wix** — the **free plan works** (shows small Wix ads); paid *Light* (~$17/mo) removes them. Use an *Embed → Embed a Code* element. Must be an `https` link (GitHub Pages already is).

---

## Updating the numbers (e.g. new total)

No file uploads needed — the numbers live in the embed link:

1. Open the **builder**, type the new School Year / Goal / Raised amounts.
2. Click **Copy embed code**.
3. In your website editor, open the embed block and **replace the old embed code** with the new block.
4. Save. Done — the widget shows the new numbers immediately.

---

## Advanced: changing the built-in defaults (optional)

The defaults are what the widget shows if it's ever opened with **no** parameters. You rarely need to touch these, but to change them:

1. Open the **builder**, set the numbers, and use **Advanced → Download** to save a fresh `fundraising_thermometer.html`.
2. On github.com at **`jnetle/mv-thermometer`**, click **Add file → Upload files**, drag the new file in (replaces the old one), and **Commit changes**.
3. Wait ~1 minute (hard-refresh with Cmd+Shift+R if needed).

---

## First-time setup (already done, for reference)

The repo was created and published with:
```
git init -b main
git add .
git commit -m "Initial commit"
gh repo create mv-thermometer --public --source=. --push
# then GitHub Pages enabled: Settings → Pages → Deploy from branch → main → /(root)
```

---

## Maintenance note for engineers

`thermometer_builder.html` contains a **tokenized snapshot** of `fundraising_thermometer.html` (embedded in a hidden `<textarea id="tpl">`, with the three constants replaced by `__SCHOOL_YEAR__`, `__GOAL_AMOUNT__`, `__CURRENT_AMOUNT__`). If you change the widget itself (new background image, animation tweak), you must regenerate that snapshot in the builder. See `CONTEXT.md`.
