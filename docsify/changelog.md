# Change Log

A running history of major changes made to this site, newest first.

## Design & content

- Switched the default font stack multiple times, settling on **DM Sans** (body + headings) and
  **DM Mono** (labels/dates) site-wide, after earlier iterations using Inter + Space Grotesk +
  IBM Plex Mono, then an all-IBM-Plex stack, then briefly DM Sans + Fraunces.
- Set **light mode as the default** theme (previously dark mode was default).
- Added a **Master of Business Administration (MBA)**, Bharati Vidyapeeth Deemed University
  (Feb 2023 – Feb 2025), to the Education section.
- Removed the "How I Structure Content" and "Before & After" illustrative writing samples from
  the "How I Work" section, keeping just the five-step process flow.
- Removed all em-dashes and stylistic/compound hyphens from visible copy site-wide, while
  preserving hyphens that are part of official standard names (ASD-STE100, S1000D).
- Removed a small decorative dash that appeared before every section's "eyebrow" label.
- Removed the pulsing glow dot from the "Ask AI about me" launcher button.
- Added an **AI agents** offer card to "What I Offer," covering custom agents for peer review,
  grammar checking, and style guide enforcement.
- Restored the **Documentation** nav link (pointing to this Docsify site) after it was
  unintentionally dropped during a file sync — see the note in
  [Troubleshooting](troubleshooting.md#a-manually-added-feature-eg-a-nav-button-disappeared-after-an-update).

## Navigation & interaction

- Rebuilt the whole site from a documentation-portal aesthetic → an Apple-style offer-first
  layout → the current cinematic dark/light glacial aesthetic.
- Added a hamburger menu, initially mobile-only, later made visible on desktop too, with its
  contents changed from full section links to four direct contact links (LinkedIn, Writing
  Sample, Email, Phone).
- Fixed the hamburger toggle when it initially failed to open/close, and repositioned it to sit
  to the right of "Get in Touch."
- Converted the AI chat panel from a small floating box into a full-height **right-side sliding
  drawer** with a backdrop overlay, closable via an X button, backdrop click, or `Esc`.
- Hid the browser's visible scrollbar site-wide while keeping scroll functionality intact.
- Added a **side dot navigation** on desktop that appears only after scrolling past the hero
  banner, and highlights the current section.
- Added a **light/dark mode toggle** button next to "Get in Touch."

## AI chat widget

- Built the chat widget from scratch: floating launcher button, chat drawer, suggestion chips,
  typing indicator, and message history.
- Chose **Google Gemini's free tier** (via a Cloudflare Worker acting as a secure backend) over
  building a no-AI static FAQ bot or a direct-from-browser API call, since a direct call would
  have exposed the API key publicly.
- Fixed a **404 model error** after Google retired the originally-used model version, switching
  to the auto-updating `gemini-flash-latest` alias.
- Fixed **vague/unhelpful chatbot answers** by strengthening the system prompt with an explicit
  specialization summary and stricter instructions against hedging, and by raising the output
  token limit.
- Added automatic **retry-with-backoff** for transient 503 "model overloaded" errors so brief
  Gemini capacity issues don't surface as failures to visitors.

## Deployment

- Deployed the site to **GitHub Pages** from a public repository.
- Documented (and hit in practice) the free-tier limitation that GitHub Pages does not serve
  sites from private repositories.
- Documented custom domain setup via DNS A/CNAME records for a future custom domain.
- Integrated a **Docsify-powered documentation site** (this one) linked from the main portfolio's
  "Documentation" button.
