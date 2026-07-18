# Troubleshooting

Real issues encountered while building and maintaining this site, and how they were resolved.

## The chatbot says "Sorry, something went wrong"

This is a generic fallback message — it means the Worker responded, but something failed either
in the Worker itself or in its call to Gemini. To find the real cause:

1. Open the live site and open the chat drawer.
2. Open browser Developer Tools (`F12`, or right-click → Inspect).
3. Click the **Network** tab.
4. Ask the chatbot a question.
5. Find the request to the `.workers.dev` URL, click it, and check its **Response** tab.
6. The JSON shown there contains the actual error.

## Error: "This model ... is no longer available to new users" (404)

Google periodically retires specific Gemini model versions. Open `worker.js` and check the
`model` variable — it should be set to an auto-updating alias like `gemini-flash-latest` rather
than a specific version string, to reduce how often this breaks. If it still 404s, search for
Gemini's current model names and update the value, then redeploy the Worker.

## Error: "This model is currently experiencing high demand" (503, UNAVAILABLE)

This is a temporary Gemini capacity issue, not a bug in this project. It's more common with
`-latest` aliases since they route to newer, higher-demand models. The Worker automatically
retries the request up to three times with a short delay before giving up — if it still fails
after that, it's a transient issue and the visitor should just try again shortly after.

## The chatbot gives vague or unhelpful answers

This is a prompt-quality issue, not a bug. Add more explicit, specific facts to
`PORTFOLIO_CONTEXT` in `worker.js` covering that exact topic, and redeploy. The more directly a
fact is spelled out, the more directly the model will answer with it. Broad questions
(e.g. "what does he specialize in") need an explicit summary written for them — the model won't
reliably synthesize one on its own from scattered facts.

## The chatbot doesn't respond at all / CORS-related errors

- Confirm the `WORKER_URL` in `index.html` exactly matches the real Worker URL, with no typos.
- Confirm the Worker was actually redeployed after any edits — changes made in Cloudflare's
  editor don't take effect until **Save and Deploy** is clicked.

## GitHub Pages site went offline after a change

- Confirm the repository is still set to **Public** — GitHub Pages on a free account does not
  serve sites from private repositories (see [Deployment](deployment.md)).
- Check **Settings → Pages** to confirm the source branch/folder is still `main` / `/ (root)`.
- Allow a minute or two — deploys are not instant.

## A manually-added feature (e.g. a nav button) disappeared after an update

This happens when a locally-edited copy of `index.html` is pasted over the live version, silently
overwriting anything that was changed directly on GitHub in between. Before pasting a new full
version of `index.html` over the live one, check whether anything was manually added on GitHub
since the last sync, and carry it forward into the new version first.

## Hamburger menu icon doesn't open/close

Confirm the button and panel share matching `id` attributes referenced in the JavaScript
(`aiLauncher` / `aiPanel` for the AI widget, `navToggle` / `mobileLinks` for the nav menu), and
that `aria-expanded` is being toggled correctly — the icon's animation and the panel's visibility
both key off that attribute.
