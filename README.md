# Abhishek Bhosale — Portfolio / Resume Site

An interactive, single-page resume site with a built-in AI chat assistant that answers visitor
questions about Abhishek's career. Built as plain HTML/CSS/JavaScript (no frameworks), hosted
free on GitHub Pages, with a small serverless backend (Cloudflare Worker) powering the AI chat.

**Live site:** _add your GitHub Pages / custom domain URL here once deployed_

This README is written so that someone with **zero prior experience** can rebuild, redeploy, or
maintain this project from scratch. Every step is spelled out — nothing is assumed.

---

## Table of Contents

1. [What's in this repo](#whats-in-this-repo)
2. [How the site is built](#how-the-site-is-built)
3. [Running the site locally](#running-the-site-locally)
4. [Deploying to GitHub Pages](#deploying-to-github-pages)
5. [Custom domain setup](#custom-domain-setup)
6. [How the AI chat widget works](#how-the-ai-chat-widget-works)
7. [Setting up the AI chat widget from scratch](#setting-up-the-ai-chat-widget-from-scratch)
8. [Updating content on the site](#updating-content-on-the-site)
9. [Updating what the AI assistant knows](#updating-what-the-ai-assistant-knows)
10. [Light / dark mode](#light--dark-mode)
11. [Troubleshooting](#troubleshooting)
12. [Keeping the repo public vs private](#keeping-the-repo-public-vs-private)
13. [Contact](#contact)

---

## What's in this repo

```
.
├── index.html          # The entire site: markup, styles, and scripts in one file
├── profile-photo.png   # Profile photo shown in the hero banner
├── worker.js            # Backend code for the AI chat widget (deployed separately on Cloudflare)
└── README.md            # This file
```

**Important:** `worker.js` is kept in this repo purely for reference/documentation. It is **not**
automatically deployed from GitHub — it only works once you've manually pasted its contents into
a Cloudflare Worker (see [Section 7](#setting-up-the-ai-chat-widget-from-scratch)). Editing this
file in GitHub does nothing to the live chatbot until you also update it in the Cloudflare
dashboard.

---

## How the site is built

- **HTML5 / CSS3** — all styling uses CSS custom properties (variables) so the entire site can
  switch between light and dark themes by changing one attribute (`data-theme`) on the `<html>` tag.
- **Vanilla JavaScript** — no React, no build step, no npm install. Everything runs directly in
  the browser.
- **Fonts** — Inter, Space Grotesk, and IBM Plex Mono, loaded from Google Fonts.
- **No backend for the resume content itself** — all your career data (roles, skills, education,
  etc.) is written directly into `index.html`. There is no database or CMS.
- **One small backend for the AI chat only** — a Cloudflare Worker (serverless function) that
  calls Google's Gemini AI API on the visitor's behalf, so your API key never has to sit inside
  the public HTML.

---

## Running the site locally

You don't need any of this to just preview the site — but it's useful if you're editing and want
to check your changes before pushing them live.

**Simplest option:** just double-click `index.html` and it opens in your browser directly.

**Better option (recommended):** serve it through a local server so all features behave exactly
like they will online:

1. Install [Python](https://www.python.org/downloads/) if you don't already have it (most Macs
   and Linux machines already do).
2. Open a terminal, navigate to the folder containing `index.html`:
   ```bash
   cd path/to/your/folder
   ```
3. Run:
   ```bash
   python3 -m http.server 8000
   ```
4. Open your browser to `http://localhost:8000`

---

## Deploying to GitHub Pages

This is how the site gets from a file on your computer to a real, public web address.

### First-time setup

1. Go to [github.com](https://github.com) and log in (or create a free account).
2. Click the **+** icon in the top-right corner → **New repository**.
3. Name it (e.g. `abhishek_portfolio`), set visibility to **Public**, then **Create repository**.
   - Public is required for GitHub Pages to work on a free account — see
     [Section 12](#keeping-the-repo-public-vs-private) for why.
4. On the new repo's page, click **"uploading an existing file"** (or drag and drop).
5. Upload `index.html` and `profile-photo.png` (keep both — the photo is referenced by the HTML).
6. Click **Commit changes**.
7. Go to the repo's **Settings** tab (top menu) → **Pages** (left sidebar).
8. Under **"Build and deployment" → Source**, choose **Deploy from a branch**.
9. Under **Branch**, select `main` and `/ (root)`, then click **Save**.
10. Wait about 1–2 minutes. Your site will be live at:
    ```
    https://<your-username>.github.io/<repo-name>/
    ```

### Updating the site after the first deploy

Any time you want to change something on the live site:

1. Go to your repo on github.com.
2. Click on the file you want to change (usually `index.html`).
3. Click the **pencil icon** (✏️) in the top-right of the file view to edit it directly in the browser.
4. Make your changes (or select-all, delete, and paste in a full replacement file).
5. Scroll down and click the green **"Commit changes"** button.
6. Wait 30–60 seconds, then refresh your live site — the change will be there.

---

## Custom domain setup

If you own a domain (e.g. `abhishekbhosale.com`) and want the site to live there instead of the
`.github.io` address:

1. In your repo: **Settings → Pages**.
2. Under **"Custom domain,"** type your domain (e.g. `abhishekbhosale.com`) → **Save**.
   GitHub automatically creates a `CNAME` file in your repo — leave it alone.
3. Go to your domain registrar (wherever you bought the domain — Namecheap, GoDaddy, etc.) and
   open its **DNS settings** page.
4. Add these records:

   **For the bare/apex domain** (`abhishekbhosale.com`, no `www`) — add four **A records**, each
   pointing to one of these IP addresses:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```

   **For the `www` version** (`www.abhishekbhosale.com`) — add one **CNAME record**:
   ```
   www  →  <your-username>.github.io
   ```

5. Wait for DNS to propagate (10 minutes to 24 hours, usually well under an hour).
6. Back in **Settings → Pages**, once GitHub shows a green checkmark next to your domain, tick
   **"Enforce HTTPS"**.

---

## How the AI chat widget works

There's a floating **"Ask AI about me"** button on the site. Clicking it opens a chat drawer where
visitors can ask questions like *"What does he specialize in?"* or *"How do I contact him?"* and
get an AI-generated answer based only on real facts about Abhishek.

Here's the flow, in plain terms:

```
Visitor types a question in the chat box
        ↓
Browser sends the question to a Cloudflare Worker (a small server-side script)
        ↓
The Worker adds a "system prompt" containing all of Abhishek's real career facts,
then forwards the question + facts to Google's Gemini AI API
        ↓
Gemini generates an answer using only those facts
        ↓
The Worker sends that answer back to the browser
        ↓
The chat window displays it
```

**Why not call Gemini directly from the browser?** Because that would require putting a secret
API key directly into public HTML — anyone could open the browser's developer tools, steal the
key, and rack up charges on your account. The Cloudflare Worker exists specifically to keep that
key private on a server, while still letting the public website use it indirectly.

---

## Setting up the AI chat widget from scratch

If you ever need to rebuild this (new Cloudflare account, key got compromised, starting over,
etc.), here is the **complete** process.

### Step 1: Get a free Gemini API key

1. Go to [aistudio.google.com](https://aistudio.google.com) and sign in with a Google account.
2. Accept the Generative AI Terms of Service if prompted.
3. In the left sidebar, click **"Get API key"** (or go directly to
   [aistudio.google.com/apikey](https://aistudio.google.com/apikey)).
4. Click **"Create API key"**.
5. Choose **"Create API key in a new project"** (the simplest option).
6. Copy the key that appears (it starts with `AIza...`) and save it somewhere safe temporarily —
   you'll paste it into Cloudflare in Step 3.
7. **Security step:** on the API Keys page, if your new key shows an **"Unrestricted"** label,
   hover over it → **"Add restrictions"** → select **"Restrict to Gemini API only"** → confirm.
   This limits what the key can be used for if it's ever exposed.

### Step 2: Create a Cloudflare account and a Worker

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → sign up (free, no credit card
   required).
2. In the left sidebar, click **Workers & Pages**.
3. Click **Create** → **Create Worker**.
4. On the "Create a Worker" screen, select **"Start with Hello World!"** (this gives you a blank
   starter — don't pick "Connect GitHub," "Select a template," or "Upload your static files,"
   those are for different use cases).
5. Give it a name (e.g. `abhishek-portfolio-ai`) → click **Deploy**. This deploys a placeholder
   script first; you'll replace it next.

### Step 3: Paste in the Worker code

1. On your new Worker's page, click **Edit code**.
2. Select all the existing placeholder code and delete it.
3. Open `worker.js` from this repo, copy its entire contents.
4. Paste it into the Cloudflare code editor.
5. Click **Save and Deploy**.

### Step 4: Add your Gemini API key as a secret

Your API key must never be typed directly into the code — it goes in as a separate, encrypted
**secret** instead.

1. On your Worker's page, go to **Settings → Variables and Secrets**.
2. Click **Add**.
3. Set:
   - **Type:** `Secret` (not "Text"/"Plaintext" — Secret encrypts the value so it can't be viewed
     again after saving, only replaced)
   - **Variable name:** `GEMINI_API_KEY` (must match exactly — this is what the code looks for)
   - **Value:** paste the API key you copied in Step 1
4. Click **Save** (or **Deploy**, depending on the Cloudflare interface at the time).

### Step 5: Get your Worker's URL

1. On your Worker's overview page, you'll see its URL — something like:
   ```
   https://abhishek-portfolio-ai.<your-subdomain>.workers.dev
   ```
2. Copy this whole URL.

### Step 6: Point the website at your Worker

1. Open `index.html`.
2. Search for this line (near the bottom, inside the `<script>` section, in the
   `// AI chat widget` block):
   ```js
   var WORKER_URL = "https://abhishek-portfolio.abhishekbhosale12.workers.dev/";
   ```
3. Replace the URL in quotes with **your own** Worker URL from Step 5.
4. Save the file and redeploy it to GitHub Pages (see
   [Updating the site after the first deploy](#updating-the-site-after-the-first-deploy)).

### Step 7: Test it

1. Open your live site.
2. Click the **"Ask AI about me"** button.
3. Type a question and send it.
4. If it works, you'll get an AI-generated answer within a couple of seconds.
5. If it doesn't, see [Troubleshooting](#troubleshooting) below.

---

## Updating content on the site

All the resume content — job history, skills, education, etc. — lives directly inside
`index.html`, organized into `<section>` blocks with clear IDs:

| Section | Look for `id=` |
|---|---|
| Hero banner | `id="overview"` |
| What I Offer | `id="offer"` |
| Experience | `id="experience"` |
| How I Work | `id="process"` |
| Proof of Work | `id="proof"` |
| Toolkit | `id="toolkit"` |
| Education | `id="education"` |
| Contact | `id="contact"` |

To edit, open the file, use your browser's or text editor's **Find** function (Ctrl+F / Cmd+F) to
jump to the relevant `id=`, and edit the text between the HTML tags directly. There's no separate
data file — it's all inline.

**After editing:** save the file and push it to GitHub the same way as before (see
[Deploying to GitHub Pages](#deploying-to-github-pages)).

---

## Updating what the AI assistant knows

The AI chatbot does **not** automatically read your website's content — it only knows what's
written inside `worker.js`, in a block near the top called `PORTFOLIO_CONTEXT`. If you update your
resume on the site but don't update this block, the chatbot will keep giving outdated answers.

To update it:

1. Open `worker.js`.
2. Find the `PORTFOLIO_CONTEXT` block near the top — it's a plain-English list of facts about
   roles, skills, initiatives, education, and contact info.
3. Edit the relevant facts (add a new job, update a skill list, etc.) directly as plain text —
   no special formatting needed, just clear, factual sentences.
4. Copy the entire updated file.
5. Go to your Cloudflare Worker → **Edit code** → select all → delete → paste the updated version
   → **Save and Deploy**.
6. Test the chatbot again to confirm it reflects the update.

**Also update the copy of `worker.js` in your GitHub repo** so the two stay in sync for your own
reference (this step doesn't affect the live chatbot — see the note in
[What's in this repo](#whats-in-this-repo) — but it keeps things tidy).

---

## Light / dark mode

The site defaults to **light mode**. Visitors can switch to dark mode using the moon/sun icon
button in the top navigation bar, next to "Get in Touch." The choice does not persist between
page visits (it resets to light mode on every fresh page load) — there's no login system or
storage to remember a preference per-visitor.

To change the default theme yourself, open `index.html` and find this line near the very top:
```html
<html lang="en" data-theme="light">
```
Change `"light"` to `"dark"` to make dark mode the default instead.

---

## Troubleshooting

### The chatbot replies "Sorry, something went wrong..."

This means the Worker responded, but something failed either in the Worker itself or in the call
to Gemini. To see the exact reason:

1. Open your live site.
2. Open your browser's Developer Tools (`F12`, or right-click → **Inspect**).
3. Click the **Network** tab.
4. Ask the chatbot a question again.
5. Find the request to your `.workers.dev` URL in the list, click it, then check its
   **Response** tab.
6. Read the error message shown there — it will tell you exactly what went wrong (see common
   causes below).

### Error: "This model ... is no longer available to new users" (404 error)

Google periodically retires specific Gemini model versions. Open `worker.js`, find this line:
```js
const model = "gemini-flash-latest";
```
This uses Google's auto-updating alias, which should avoid this issue going forward. If it
still 404s, search "Gemini API current model names" to find the latest valid model ID and
replace the value above with it, then redeploy the Worker (Cloudflare → Edit code → paste →
Save and Deploy).

### The chatbot doesn't respond at all / CORS errors in the console

- Double check the `WORKER_URL` in `index.html` exactly matches your Worker's real URL (including
  `https://` and the trailing details, no typos).
- Make sure the Worker was actually redeployed after any edits (edits made in Cloudflare's
  editor don't take effect until you click **Save and Deploy**).

### The chatbot gives vague or unhelpful answers to reasonable questions

Add more explicit facts to `PORTFOLIO_CONTEXT` in `worker.js` covering that specific topic, then
redeploy. The more directly a fact is spelled out, the more directly the AI will answer with it.

### GitHub Pages site isn't showing up / went offline after a change

- Confirm the repository is set to **Public** (see next section for why).
- Check **Settings → Pages** to make sure the source branch/folder is still set to `main` /
  `/ (root)`.
- Give it a minute or two — deploys aren't instant.

---

## Keeping the repo public vs private

On a **free personal GitHub account**, GitHub Pages **only works from public repositories**. If
you switch this repo to private, your live site will go offline immediately — there's no
in-between "private but still hosted" option on the free tier.

- If you want to hide the *code* but keep the *live site* working, that requires a paid GitHub
  Pro plan — and even then, the published site itself is still publicly visible to anyone with
  the link (Pro only hides the source code, not the rendered page).
- Since a resume is meant to be seen, the simplest and free-tier-friendly choice is to **keep this
  repo public**.

Your Gemini API key is safe either way — it's never stored in this repo at all, only inside
Cloudflare's encrypted secrets.

---

## Contact

- Email: [abhishekbhosale12@gmail.com](mailto:abhishekbhosale12@gmail.com)
- Phone: +91 91688 49070
- Location: Pune, India
- LinkedIn: [linkedin.com/in/abhishek-bhosale-0aa10383](https://www.linkedin.com/in/abhishek-bhosale-0aa10383)
