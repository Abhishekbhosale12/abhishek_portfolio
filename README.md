# Abhishek Bhosale — Portfolio / Resume Site

An interactive, single-page resume site built for Abhishek Bhosale, Technical Writer & Information Developer. Instead of a static PDF, this site presents career history, capabilities, and shipped work as a scrollable, animated experience aimed at hiring managers and recruiters.

**Live site:** _add your GitHub Pages / custom domain URL here once deployed_

---

## What's inside

- **Hero banner** — name, role, photo, and a one-line value statement
- **What I Offer** — capability cards framed around outcomes for hiring teams (documentation velocity, cross-functional collaboration, AI-assisted QA agents, and more)
- **Experience** — every role in full detail, presented as an expandable changelog (click to open/collapse)
- **Proof of Work** — shipped initiatives with links to live pages where available
- **Toolkit** — skills and tools, filterable by category
- **Education & Certifications**
- **Contact** — LinkedIn, email, and phone, available from the top toolbar, mobile menu, and closing call-to-action

## Features

- Light / dark mode toggle
- Smooth scroll-triggered animations
- Fully responsive layout (desktop, tablet, mobile)
- No frameworks or build step — plain HTML, CSS, and JavaScript
- No external dependencies besides Google Fonts

## Tech stack

- HTML5 / CSS3 (custom properties for theming)
- Vanilla JavaScript (no libraries or frameworks)
- Google Fonts: Inter, Space Grotesk, IBM Plex Mono

## File structure

```
.
├── index.html          # Full site — markup, styles, and scripts in one file
├── profile-photo.png   # Profile photo used in the hero banner
└── README.md
```

## Running locally

No build tools or dependencies required. Either:

- Open `index.html` directly in a browser, or
- Serve it locally for the best experience:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploying with GitHub Pages

1. Push `index.html` and `profile-photo.png` to this repository (keep them in the same folder — the photo is referenced by relative path).
2. Go to **Settings → Pages**.
3. Under **Build and deployment → Source**, select **Deploy from a branch**.
4. Choose the `main` branch and `/ (root)` folder, then save.
5. The site will be live at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

> Note: On a free GitHub account, Pages only works when the repository is **public**. A private repo will not serve the site unless you're on GitHub Pro or higher.

### Custom domain (optional)

1. In **Settings → Pages**, add your domain under "Custom domain."
2. At your domain registrar, add DNS records pointing to GitHub Pages:
   - **A records** (apex domain) → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **CNAME record** (`www` subdomain) → `<your-username>.github.io`
3. Wait for DNS propagation, then enable **Enforce HTTPS** in the Pages settings.

## Updating content

All content lives directly in `index.html` — there's no CMS or data file to edit separately. Look for the relevant `<section>` (`id="offer"`, `id="experience"`, `id="proof"`, `id="toolkit"`, `id="education"`, `id="contact"`) and edit the markup in place.

## Contact

- Email: [abhishekbhosale12@gmail.com](mailto:abhishekbhosale12@gmail.com)
- Phone: +91 91688 49070
- LinkedIn: [linkedin.com/in/abhishek-bhosale-0aa10383](https://www.linkedin.com/in/abhishek-bhosale-0aa10383)
