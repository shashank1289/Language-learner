# Side by Side: English–Korean reader (self-hosted, Gemini)

English on the left, Korean on the right, with audio for the English.
Upload a PDF, EPUB or TXT book in English or Korean, and Gemini fills in
the other language sentence by sentence.

Files:
- `index.html` is the whole app (one file, no build step)
- `worker.js` is an optional server that hides your Gemini key (Option B)

## 1. Put the site online (GitHub Pages, free)

1. Create a new public repository on github.com, for example `english-reader`.
2. Upload `index.html` to it (Add file, then Upload files, then Commit).
3. Open Settings, then Pages. Under "Branch" choose `main` and `/ (root)`, then Save.
4. After about a minute your site is live at
   `https://<your-username>.github.io/english-reader/`

Netlify or Vercel also work: drag the folder onto their dashboard.

## 2. Turn on translation

### Option A: each person uses their own key (simplest)

Nothing to set up. Each visitor opens **Translation settings**, pastes a
free key from https://aistudio.google.com/apikey, and presses **Test**,
then **Save**. The key stays in their browser and is sent only to Google.

### Option B: one shared key on a small server (for sharing with others)

Visitors don't need a key; your key stays secret on Cloudflare.

1. Sign in at https://dash.cloudflare.com (free plan is fine).
2. Go to Workers & Pages, then Create, then Create Worker. Name it and deploy.
3. Press **Edit code**, replace everything with `worker.js`, and deploy.
4. In the Worker's Settings, then Variables and Secrets, add:
   - `GEMINI_API_KEY` as a **Secret**: your Gemini key
   - `ALLOWED_ORIGIN` as Text: your site's origin, e.g. `https://yourname.github.io`
     (no path, no trailing slash)
5. Copy the Worker URL (like `https://english-reader.yourname.workers.dev`).
6. In `index.html`, find `PROXY_URL: ""` near the top of the script and
   paste the URL between the quotes. Upload the changed file to GitHub.

Everyone who uses the site now spends **your** Gemini quota. Keep an eye
on usage in Google AI Studio, and set a budget alert if you're on a paid plan.

## Changing the model

`gemini-flash-latest` always points to Google's newest Flash model.
To use another one (for example a Pro model for higher quality), change
it in Translation settings, or change `DEFAULT_MODEL` in `index.html`.

## Good to know

- Uploaded books are stored in each person's browser (IndexedDB), not on a server.
- Scanned PDFs, DRM-protected ebooks and Kindle files can't be read. Use DRM-free EPUBs.
- Audio uses the device's built-in voices; Chrome, Safari and Edge have the best ones.
