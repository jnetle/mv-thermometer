# Hosting & Updating the Thermometer

This project is hosted free on **GitHub Pages**. Two files live in the repo:

| File | What it's for | Lives at |
|------|---------------|----------|
| `fundraising_thermometer.html` | The widget visitors see. **This is the one you embed.** | `https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html` |
| `thermometer_builder.html` | Your private control panel for generating the widget. Not embedded anywhere. | `https://jnetle.github.io/mv-thermometer/thermometer_builder.html` |

> The repo is **public** (required for free GitHub Pages). Nothing sensitive is in it, so that's fine.

---

## Embedding it on a website (Wix or Squarespace)

1. Open the **builder** (link above) and bookmark it.
2. In the builder's **"Embed snippet"** box, paste the widget URL:
   `https://jnetle.github.io/mv-thermometer/fundraising_thermometer.html`
3. Click **Copy embed code** — you get one `<iframe>` line.
4. In your site editor, add an **Embed / HTML block** and paste it.

**Plan notes:**
- **Squarespace** — needs the **Core plan ($23/mo)** or higher; the cheapest Basic plan won't run the animation. Use an *Embed Block*.
- **Wix** — the **free plan works** (shows small Wix ads); paid *Light* (~$17/mo) removes them. Use an *Embed → Embed a Code* element. Must be an `https` link (GitHub Pages already is).

The embed link never changes, so once it's in, you never touch your website again.

---

## Updating the numbers (e.g. new year or new total)

1. Open the **builder**, type the new School Year / Goal / Raised amounts, watch the preview.
2. Click **Download** to save a fresh `fundraising_thermometer.html`.
3. Go to the repo on github.com: **`jnetle/mv-thermometer`**.
4. Click **Add file → Upload files**, drag in the new `fundraising_thermometer.html` (it replaces the old one), then **Commit changes**.
5. Wait ~1 minute. The live widget updates automatically. (If you don't see it, hard-refresh: Cmd+Shift+R.)

That's it — no website edits, no code.

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
