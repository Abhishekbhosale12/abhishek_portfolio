# Site Overview

The portfolio is a single HTML file (`index.html`) organized into distinct sections, each with
its own `id` so they can be linked to directly and targeted by navigation.

## Sections, in order

| Section | `id` | What it contains |
|---|---|---|
| Hero banner | `overview` | Name, role, one-line value statement, profile photo, and primary call-to-action buttons |
| What I Offer | `offer` | Capability cards framed around outcomes for hiring teams — not a duties list |
| How I Work | `process` | A five-step visual of the documentation workflow, from feature discussion to publish |
| Proof of Work | `proof` | Shipped initiatives, with links to live pages where available |
| Toolkit | `toolkit` | Skills and tools, filterable by category (Authoring, Tools, Style, Emerging) |
| Education | `education` | Degrees, diploma, and certifications |
| Contact | `contact` | Closing call-to-action with email, phone, and LinkedIn |

## Navigation

- **Top navigation bar** — sticky, blurred background, shrinks visual weight on scroll. Contains:
  - Desktop-only icon links: Documentation, LinkedIn, Writing Sample, Email, Phone
  - The light/dark mode toggle
  - A "Get in Touch" button
  - A hamburger menu (mobile and desktop) that opens a small drawer with the same four contact
    links, for quick access without needing to scroll to the bottom
- **Side dot navigation** — appears on desktop only, after scrolling past the hero banner. Shows
  which section is currently in view and lets visitors jump between sections.

## Interactive elements

- **Scroll-reveal animations** — sections and cards fade and slide into view as the visitor
  scrolls, using the Intersection Observer API. Respects the visitor's OS-level "reduce motion"
  setting.
- **Expandable experience entries** — each role in the Experience section is collapsed by
  default, showing just the title, company, and dates. Clicking expands it to show full
  responsibilities. The current role is expanded by default.
- **Filterable toolkit** — clicking a category tab (Authoring, Tools, Style, Emerging) filters
  the skill pills shown below it.
- **AI chat widget** — a floating button that opens a right-side drawer where visitors can ask
  questions about Abhishek's background. Covered in detail in
  [AI Chat Widget](ai-chat-widget.md).
- **Light/dark mode toggle** — switches the whole site's color scheme instantly. Covered in
  [Design System](design-system.md).

## What's intentionally not here

- No backend or database for the resume content — everything is hand-written directly into
  `index.html`.
- No CMS — updating content means editing HTML directly (see
  [Updating Content](updating-content.md)).
- No build step, bundler, or package manager — the file can be opened directly in a browser with
  zero setup.
