# Portfolio Project Documentation

This is the technical documentation for **abhishekbhosale12.github.io/abhishek_portfolio** —
Abhishek Bhosale's interactive resume site. It covers how the site is built, how it's deployed,
and how every feature on it works, so it can be maintained, rebuilt, or extended by anyone
(including a future version of Abhishek) without needing to guess.

## What this site is

A single-page, interactive resume built with plain HTML, CSS, and JavaScript — no frameworks,
no build tools. It presents career history, skills, and shipped work as a scrollable experience
for hiring managers and recruiters, and includes an AI chat assistant that can answer visitor
questions about Abhishek using real facts pulled from his career.

## What's in here

- **[Site Overview](site-overview.md)** — what's on the page, section by section
- **[Design System](design-system.md)** — fonts, colors, light/dark mode, and how theming works
- **[Deployment](deployment.md)** — hosting on GitHub Pages, and setting up a custom domain
- **[AI Chat Widget](ai-chat-widget.md)** — how the chatbot works, and how to set it up from scratch
- **[Updating Content](updating-content.md)** — how to edit the resume content or what the AI knows
- **[Troubleshooting](troubleshooting.md)** — fixes for issues that have come up during development
- **[Change Log](changelog.md)** — a running history of every major change made to this site

## Repository structure

```
.
├── index.html          # The entire site — markup, styles, and scripts in one file
├── profile-photo.png   # Profile photo shown in the hero banner
├── worker.js            # Backend code for the AI chat widget (deployed on Cloudflare, not GitHub)
├── README.md            # Top-level project README
└── docsify/              # This documentation site
```

---

Looking for the resume itself, not the documentation about it? Go back to the
[main portfolio site](../index.html).
