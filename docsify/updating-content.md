# Updating Content

There are two completely separate things that can be "updated" on this site, and they live in
different files. Mixing them up is the most common source of confusion.

## 1. The resume content itself

Everything visible on the site — job history, skills, education, initiatives — is written
directly inside `index.html`. There is no CMS or external data file.

| Section | Look for `id=` in `index.html` |
|---|---|
| Hero banner | `id="overview"` |
| What I Offer | `id="offer"` |
| Experience | `id="experience"` |
| How I Work | `id="process"` |
| Proof of Work | `id="proof"` |
| Toolkit | `id="toolkit"` |
| Education | `id="education"` |
| Contact | `id="contact"` |

To edit: open `index.html`, use Find (Ctrl+F / Cmd+F) to jump to the relevant `id=`, and edit the
text between the HTML tags directly.

After editing, redeploy by committing the change to GitHub (see [Deployment](deployment.md)).

## 2. What the AI chatbot knows

The AI widget does not read `index.html` at all. It only knows what's written in the
`PORTFOLIO_CONTEXT` block inside `worker.js`.

**If you update the resume but not this block, the chatbot will keep giving outdated answers.**

To update it:

1. Open `worker.js`.
2. Edit the plain-English facts inside `PORTFOLIO_CONTEXT`.
3. Copy the full updated file.
4. Paste it into the Cloudflare Worker's code editor (Edit code → select all → paste).
5. **Save and Deploy.**
6. Test the chatbot to confirm it reflects the change.

Also update the copy of `worker.js` kept in this GitHub repo, so both stay in sync for your own
reference — though this step alone does not affect the live chatbot (see
[AI Chat Widget](ai-chat-widget.md) for why).

## Adding a new section to the site

If a new section needs to be added (not just edited):

1. Copy the structure of an existing `<section>` block as a starting point.
2. Give it a new, unique `id`.
3. Add a matching entry to both the side dot navigation and the mobile drawer navigation (search
   for `side-nav` and `mobile-links` in `index.html`).
4. If it should also be searchable/answerable by the AI chatbot, add relevant facts to
   `PORTFOLIO_CONTEXT` in `worker.js` as well.
