# Apartmán u Mešťana — Editing Guide

Where to change things in `index.html`. Use **Ctrl+F** to find the exact strings shown below.

---

## 🔢 Quick reference — what to change where

| What you want to change | Where to look (search string) | Type |
|---|---|---|
| Default guest PIN | `const DEFAULT_PIN` | JS constant |
| Default WiFi network name | `const DEFAULT_WIFI_SSID` | JS constant |
| Default WiFi password | `const DEFAULT_WIFI_PASS` | JS constant |
| Default key box code | `const DEFAULT_KEYBOX` | JS constant |
| Admin access code | `const ADMIN_PIN` | JS constant |
| App version (force-reset flag) | `const APP_VERSION` | JS constant |
| Apartment name (header) | `<h1>Apartmán u Mešťana</h1>` | HTML |
| Apartment name (lock screen) | `<div class="lock-name">` | HTML |
| Apartment name (drawer) | `<div class="drawer-title">` | HTML |
| Phone number | `tel:+421...` and the displayed number | HTML, Contact section |
| Email address | `mailto:info@...` and the displayed address | HTML, Contact section |
| Physical address | Search for `Akademická 6` | HTML, Transport section |
| Arrival directions | `transportCarText` + `transportFromBa/Zv/Bb` | i18n dictionaries |
| Parking instructions | `transportParkingText` | i18n dictionaries |
| Bus/taxi info | `transportBusText`, `transportTaxiText` | i18n dictionaries |
| House rules (7 items) | Search for `ruleSmoking`, `ruleQuiet`, etc. | i18n dictionaries |
| Welcome message (home) | `heroDesc` | i18n dictionaries |
| Key box hint text | `homeKeyboxHint` | i18n dictionaries |
| All 9 local tips | `const TIPS = [` | JS array (single source) |
| Apartment photo gallery | `const GALLERY = [` | JS array (single source) |
| Recommended parking spots | `const PARKING = [` | JS array (single source) |
| Logo image | File: `logo.png` | Image file |
| Tip images | Folder: `tips/` | Image files |
| Gallery photos | Folder: `gallery/` | Image files |

---

## ✏️ Most common edits

### Change the guest PIN for a new booking

Two options:

**A — Quick (this device only):** Open app → long-press logo → enter admin code → edit "PIN hostí" → Save.

**B — Permanent (all guests, all devices):** Edit `DEFAULT_PIN` in index.html + bump `APP_VERSION` (see below) + redeploy.

### Change WiFi credentials

Same two options as above. For permanent:
```js
const DEFAULT_WIFI_SSID = "YourNetworkName";
const DEFAULT_WIFI_PASS = "YourPassword";
```

### Change phone / email

Edit directly in the HTML (Contact section). Look for:
```html
<a href="tel:+421900000000">...</a>
<a href="sms:+421900000000">...</a>
<a href="mailto:info@umestana.sk">...</a>
```
Change **both** the `href` and the displayed text inside `<div class="c-value">`.

### Change the apartment address

Search for `Akademická 6`. Replace with your address. The GPS note uses rough coordinates — update if needed.

### Edit translations (SK or EN)

All translatable strings live in two dictionaries:
- `I18N.sk = { ... }` (Slovak)
- `I18N.en = { ... }` (English)

Find the key, edit the value in BOTH dictionaries. HTML `data-i18n="keyName"` attributes are auto-populated from these dictionaries on load and when language switches.

---

## 📍 Tips (sightseeing recommendations)

All 9 tips live in one JS array near the top of the script. Search for `const TIPS = [`.

Each tip is a single object:
```js
{
  img: "tips/kalvaria.jpg",   // or null/omit for no image
  category: { sk: "UNESCO", en: "UNESCO" },
  name:     { sk: "Kalvária", en: "Calvary" },
  desc:     { sk: "...", en: "..." },
  meta:     { sk: "📍 25 min chôdze", en: "📍 25 min walk" }
}
```

**To add a tip:** copy one object, paste anywhere in the array, change all fields.
**To remove:** delete the object.
**To reorder:** move the object.
**To add an ornamental separator:** insert `{ divider: true },` where you want the ❦.

After editing text, the image path and filename stay the same — rename the image file OR update `img` path. Image not found → card still works, just no photo.

---

## 🅿️ Parking spots (Doprava section)

Recommended parking locations live in a `PARKING` array in `index.html`. Each spot shows as a small card with a "Show on map" button that opens the user's default map app.

Search for `const PARKING = [` in `index.html`:

```js
{
  name: { sk: "Parkovisko pri autobusovej stanici", en: "Bus station parking" },
  desc: { sk: "Bezplatné, väčšia kapacita.", en: "Free, larger capacity." },
  mapQuery: "Autobusová stanica Banská Štiavnica"
}
```

**`mapQuery`** can be either:
- **GPS coords** — `"48.4574,18.8916"` (most accurate). Get them by right-clicking a spot in Google Maps → *"What's here?"* → copy the numbers.
- **Or a search string** — `"Parkovisko autobusová stanica Banská Štiavnica"` (easier, but less precise).

**To add a spot:** copy one object, paste, update fields.
**To remove:** delete the object.
**To reorder:** move it.

The map button opens `google.com/maps` — on mobile this auto-launches the Google Maps app (Android) or Apple Maps/web fallback (iOS).

---

## 🖼️ Images

### Logo
Location: `logo.png` in the project root.
Used in: lock screen, drawer header, Home hero card, PWA icon, favicon.
Recommended: square, at least 512×512 px, PNG with transparent background.

### Tip images
Folder: `tips/`
Naming must match the `img:` field of each tip in the TIPS array.

Current filenames expected:
- `tips/trojice.jpg`
- `tips/kalvaria.jpg`
- `tips/novy-zamok.jpg`
- `tips/banske-muzeum.jpg`
- `tips/pivovar-erb.jpg`
- `tips/tajch-klinger.jpg`
- `tips/svaty-anton.jpg`
- `tips/pocuvadlo.jpg`
- `tips/sitno.jpg`

Recommended: 16:10 landscape, ~1200×750 px, JPG, 150–400 KB each.

### Gallery (apartment photos)
Folder: `gallery/`
Naming must match the `img:` field of each item in the `GALLERY` array.

**Default numbered slots** (currently 15 entries pre-declared):
```
gallery/01.jpg, gallery/02.jpg, ..., gallery/15.jpg
```

You don't need all 15 — just drop in as many photos as you have (e.g. only `01.jpg` through `08.jpg`). Missing files are silently hidden, so unused slots don't break anything.

**Want more than 15?** Open `index.html`, find `const GALLERY = [`, and add more `{ img: "gallery/16.jpg" },` entries.

**Recommended image specs:** square or close (1:1), ~1200×1200 px, JPG, 200–500 KB.

**Display:** thumbnail grid (2 cols on phones, 3 on wider screens) → tap to open full-screen lightbox with swipe / arrow / keyboard navigation.

**Optional captions:** add `caption: { sk: "...", en: "..." }` to any item if you want a label to appear beneath the image in the lightbox. Currently disabled — photos only.

---

## 🔐 Admin panel

**Access:** long-press (1.5 s) on any logo:
- Lock screen logo
- Home hero logo
- Drawer logo (☰ → then hold the logo)

**Admin code:** `ADMIN_PIN` constant. Currently `"1964"`.

**What you can edit:**
- Guest PIN (4 digits)
- WiFi network name
- WiFi password
- Key box code

**Buttons:**
- **Uložiť nastavenia** — saves all 4 fields to localStorage on this device
- **Obnoviť predvolené hodnoty** — two-tap confirmation; wipes localStorage and reverts to DEFAULT_* constants

---

## 🔄 APP_VERSION — force-reset all devices

Search for `const APP_VERSION`.

```js
const APP_VERSION = "1.0.0";
```

Bumping this (e.g. `"1.0.0"` → `"1.0.1"`) makes every device wipe its admin-stored values on next load and fall back to the hardcoded DEFAULT_* constants.

**When to bump:**
- You changed DEFAULT_PIN / DEFAULT_WIFI_* / DEFAULT_KEYBOX and want all guests to see the new values
- You want a clean reset after testing

**When NOT to bump:**
- Small UI tweaks, copy edits, adding tips — these don't affect stored values, so no need

---

## 🌐 Language switcher

- **Default: Slovak.** Set by `let initial = "sk";` in the init code.
- User's choice is remembered in localStorage (key: `lang`).
- Switcher appears: top-right of the header AND top-right of the lock screen.

To add more languages: add a third dictionary `I18N.de = {...}` with all the same keys, and add a third button in each `.lang-switch`. The applyLang function handles any language code you wire in.

---

## 🧭 Navigation

- **Menu:** hamburger button (☰) top-left of header opens a left-side drawer.
- **Sections:** Home, Tipy, Doprava, WiFi, Pravidlá, Kontakt.

To add a new section:

1. Add a `<section id="newSection">...</section>` inside `<main>`
2. Add a `<button data-target="newSection">...</button>` inside `<nav class="drawer-nav" id="nav">`
3. Add a translation key for the button label (e.g. `navNewSection`)
4. Add that key to both `I18N.sk` and `I18N.en`

---

## 🎨 Colors and theme

All theme colors are CSS variables at the top of the `<style>` block. Search for `:root {`.

```css
--bg: #1a130d;          /* main background */
--surface: #2e2117;     /* cards */
--gold: #c9a876;        /* accent */
--cream: #e8dfce;       /* main text */
...
```

Change these and the whole app re-themes automatically.

---

## 🚀 Deploying changes

1. Edit `index.html` in VS Code
2. Save
3. Commit + push to GitHub (if using GitHub Pages / Netlify, auto-deploys)
4. Guests refresh the app — new version loads

### Checklist before you redeploy with changed defaults
- [ ] Bumped `APP_VERSION` if you changed any DEFAULT_* constant?
- [ ] Updated translations in BOTH `I18N.sk` and `I18N.en`?
- [ ] Tested both languages (SK + EN toggle)?
- [ ] Tested on a phone (not just desktop)?

---

## 🆘 Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Admin-changed PIN didn't reset after deploy | localStorage persists | Bump `APP_VERSION` OR use the Reset button in admin |
| Tip image not showing | Filename mismatch | Check `img:` path matches the file in `tips/` exactly |
| Language doesn't switch for some text | Missing i18n key | Add the key to BOTH `I18N.sk` and `I18N.en` |
| Long-press not opening admin | Finger moved during press | Hold still for 1.5 s without dragging |
| Logo missing | File missing or wrong name | Save as `logo.png` in project root |
| Can't copy WiFi password | Browser blocks clipboard on HTTP | Deploy via HTTPS (Netlify / GitHub Pages) |

---

## 📂 Project files overview

```
Apartment-guide/
├── index.html            ← everything lives here (HTML + CSS + JS + translations)
├── logo.png              ← apartment logo
├── tips/                 ← per-tip images (optional)
│   ├── trojice.jpg
│   ├── kalvaria.jpg
│   └── ... (see TIPS array for filenames)
└── EDITING-GUIDE.md      ← this file
```

---

*Keep this file handy. For larger architectural changes (new languages, new sections, new admin fields), ask Claude to implement them — the patterns in the code are consistent and easy to extend.*
