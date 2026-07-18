# Design System

## Typography

The site uses two typefaces, both loaded from Google Fonts:

| Typeface | Used for |
|---|---|
| **DM Sans** | Everything — headings, body text, buttons, nav links, form inputs |
| **DM Mono** | Small mono-style labels: dates, section eyebrows (small uppercase tags above headings), toolkit pills, badges |

Font import (in the `<head>` of `index.html`):

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,400&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
```

> **History:** The site originally used a three-font stack (Inter for body, Space Grotesk for
> headings, IBM Plex Mono for labels), was briefly switched to an all-IBM-Plex stack, then to a
> DM Sans / Fraunces / DM Mono combination, before settling on the current DM Sans + DM Mono
> pairing for a cleaner, single-family look.

## Color system

All colors are defined as CSS custom properties on `:root`, then overridden under
`html[data-theme="light"]` for light mode. This means every component reads from the same
variable names — switching themes is just swapping which values those variables point to.

| Variable | Dark mode | Light mode | Used for |
|---|---|---|---|
| `--void` | Near-black | Off-white | Page background |
| `--void-2` | Dark slate | Light gray | Panel/card backgrounds, nav drawers |
| `--frost` | Near-white | Near-black | Primary text |
| `--mist` | Light gray-blue | Slate gray | Secondary/muted text |
| `--ice-1` / `--ice-2` / `--ice-3` | Cyan / blue / violet | Darker teal / blue / violet | Accent colors, glows, links |
| `--border` | Semi-transparent white | Semi-transparent black | Card and panel borders |
| `--on-frost` | Near-black | White | Text color used on top of `--frost`-colored buttons (inverts per theme) |

## Light / dark mode

The site defaults to **light mode**. The `<html>` tag carries a `data-theme` attribute:

```html
<html lang="en" data-theme="light">
```

Clicking the moon/sun icon in the top nav toggles this attribute between `"light"` and `"dark"`
via JavaScript — no page reload, and the whole site (backgrounds, text, borders, aurora glow
intensity) transitions smoothly because every color is a CSS variable.

**Important limitation:** the chosen theme does **not** persist between visits. Every fresh page
load resets to light mode. There's no login system or `localStorage` currently wired up to
remember a visitor's preference.

To change the default theme, edit the `data-theme` value in the `<html>` tag directly.

## Visual motifs

- **Frosted glass panels** — cards use a translucent background (`--panel`) with a soft border
  and subtle backdrop blur, giving a "glacial" layered look.
- **Aurora glow blobs** — large, blurred, softly animated radial gradients drift slowly behind
  content in the background, dimmed significantly in light mode so they don't wash out the page.
- **Cursor-reactive glow** — on desktop only, a soft radial gradient follows the mouse cursor for
  ambient depth. Disabled on touch devices and when the visitor's OS requests reduced motion.
- **Scroll-reveal animations** — sections fade and slide upward into view as they enter the
  viewport, with staggered delays for grids of cards so they don't all appear at once.
