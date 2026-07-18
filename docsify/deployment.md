# Deployment

The site is hosted for free on **GitHub Pages**, directly from this repository.

## First-time setup

1. Create a public GitHub repository.
2. Upload `index.html` and `profile-photo.png` to the repository root (keep them in the same
   folder — the photo is referenced by relative path).
3. Go to the repo's **Settings → Pages**.
4. Under **"Build and deployment" → Source**, choose **Deploy from a branch**.
5. Select the `main` branch and `/ (root)` folder, then save.
6. Within a minute or two, the site is live at:
   ```
   https://<username>.github.io/<repo-name>/
   ```

## Updating the live site

Any time content changes:

1. Open the file to change (usually `index.html`) in the GitHub web UI.
2. Click the pencil icon to edit it directly in the browser, or delete-and-paste a full
   replacement.
3. Commit the change.
4. Wait 30–60 seconds, then refresh the live site.

There is no CI/CD pipeline or automatic build process — GitHub Pages serves the raw HTML file
directly, so what you commit is exactly what visitors see.

## Custom domain

To point a custom domain (e.g. `abhishekbhosale.com`) at the site instead of the `.github.io`
address:

1. In **Settings → Pages**, add the domain under **"Custom domain"** and save. GitHub
   automatically creates a `CNAME` file in the repo.
2. At the domain registrar, add DNS records:

   **Apex domain** (`abhishekbhosale.com`) — four **A records**:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```

   **`www` subdomain** — one **CNAME record**:
   ```
   www → <username>.github.io
   ```

3. Wait for DNS propagation (10 minutes to 24 hours).
4. Once GitHub shows a green checkmark next to the domain in Settings → Pages, enable
   **"Enforce HTTPS."**

## Public vs. private repository

GitHub Pages on a **free personal account only serves sites from public repositories**. If the
repo is switched to private, the live site goes offline immediately — there's no in-between
option on the free tier.

- A paid **GitHub Pro** plan allows Pages to build from a private repo, but the published site
  itself is still publicly visible to anyone with the link. Pro only hides the source code, not
  the rendered page.
- True login-gated private hosting requires **GitHub Enterprise Cloud**, which is unnecessary
  overhead for a personal resume site.
- Since a resume is meant to be seen, and the API key powering the AI widget is never stored in
  this repo at all (see [AI Chat Widget](ai-chat-widget.md)), **keeping the repo public is both
  the simplest and the safest choice** here.
