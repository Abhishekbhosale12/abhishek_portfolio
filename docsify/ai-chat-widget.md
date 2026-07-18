# AI Chat Widget

A floating **"Ask AI about me"** button opens a chat drawer where visitors can ask questions
about Abhishek's career and get an AI-generated answer, grounded only in real facts.

## Architecture

```
Visitor types a question in the chat drawer
        ↓
Browser sends the question to a Cloudflare Worker (serverless backend)
        ↓
The Worker attaches a system prompt containing Abhishek's real career facts,
then forwards the question + facts to Google's Gemini API
        ↓
Gemini generates an answer using only those facts
        ↓
The Worker returns the answer to the browser
        ↓
The chat drawer displays it
```

**Why not call Gemini directly from the browser?** Doing so would require embedding a secret API
key inside public HTML — anyone could open developer tools, extract the key, and use it
elsewhere. The Cloudflare Worker exists specifically to keep that key server-side while still
letting the public site use it indirectly.

## Why this isn't RAG

This setup uses **static context injection**, not Retrieval-Augmented Generation (RAG). True RAG
would chunk the career data, embed it into a vector database, and retrieve only the most relevant
chunks per question. Here, the entire fact sheet (a few hundred words) is small enough to send in
full with every request — no retrieval step needed. RAG would add unnecessary complexity for a
knowledge base this size.

## Files involved

| File | Role |
|---|---|
| `worker.js` | The Cloudflare Worker's backend code. Contains the fact sheet (`PORTFOLIO_CONTEXT`) and the logic that calls Gemini. **Kept in this repo for reference only — it is not automatically deployed from GitHub.** |
| `index.html` | Contains the chat widget's markup, styling, and the JavaScript that calls the Worker's URL |

## Setting it up from scratch

### 1. Get a free Gemini API key

1. Go to [aistudio.google.com/apikey](https://aistudio.google.com/apikey) and sign in.
2. Click **"Create API key"** → **"Create API key in a new project."**
3. Copy the key (starts with `AIza...`).
4. If the key shows as "Unrestricted," add a restriction limiting it to Gemini API only.

### 2. Create a Cloudflare Worker

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and sign up (free).
2. **Workers & Pages → Create → Create Worker.**
3. Choose **"Start with Hello World!"** (not GitHub/GitLab connect, template, or static file
   upload — those are for different use cases).
4. Name it and deploy the placeholder.

### 3. Paste in the Worker code

1. On the Worker's page, click **Edit code**.
2. Delete the placeholder code.
3. Paste in the full contents of `worker.js` from this repo.
4. **Save and Deploy.**

### 4. Add the API key as a secret

1. Go to **Settings → Variables and Secrets → Add.**
2. Type: **Secret** (not Text/Plaintext — this encrypts the value).
3. Variable name: `GEMINI_API_KEY` (must match exactly).
4. Value: the Gemini key from step 1.
5. Save.

### 5. Connect the site to the Worker

1. Copy the Worker's URL from its overview page (looks like
   `https://<worker-name>.<subdomain>.workers.dev`).
2. In `index.html`, find this line inside the `// AI chat widget` script block:
   ```js
   var WORKER_URL = "https://abhishek-portfolio.abhishekbhosale12.workers.dev/";
   ```
3. Replace it with your own Worker URL.
4. Redeploy `index.html` to GitHub Pages.

## Updating what the AI knows

The chatbot does **not** read the live website automatically — it only knows what's written in
the `PORTFOLIO_CONTEXT` block at the top of `worker.js`. If the resume content changes but this
block isn't updated, the chatbot gives outdated answers.

To update it: edit the plain-English facts in `PORTFOLIO_CONTEXT`, then paste the updated file
into the Cloudflare Worker's editor and redeploy. There's no need to touch `index.html` for this.

## Known issue history

- **Model 404 errors** — Google periodically retires specific Gemini model versions. The Worker
  uses the auto-updating alias `gemini-flash-latest` instead of a hardcoded version to reduce how
  often this breaks.
- **503 "model overloaded" errors** — transient Gemini capacity issues, not a bug. The Worker
  retries automatically up to three times with a short backoff before giving up.
- **Vague or unhelpful answers** — fixed by adding an explicit "Specialization Summary" block to
  the prompt and instructing the model to answer directly and specifically rather than hedge, and
  by raising the output token limit so answers aren't cut off mid-sentence.

See [Troubleshooting](troubleshooting.md) for how to diagnose new issues if they come up.
