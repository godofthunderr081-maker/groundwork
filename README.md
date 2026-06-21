# Made-Up

**Turn a fuzzy app idea into a clean build brief your AI builder can actually execute.**

Made-Up interviews you about your idea — asking the sharp questions a senior engineer
would ask, one at a time — then writes a structured build brief you paste into
Cursor, Lovable, v0, or Claude. It fixes the prompt *before* bad code gets generated.

It's a single `index.html` file. No backend, no build step, no accounts.

---

## Run it locally

Just open the file:

- **Double-click `index.html`** (it runs straight from `file://`), or
- serve it: `python3 -m http.server` then visit `http://localhost:8000`.

On first open you'll get a short intro, then you add your API key and start.

## Get a free API key

Made-Up talks **directly** to the AI provider from your browser. Your key is stored
only in your browser (`localStorage`) and is sent only to the provider you pick —
never to any server. Pick one:

- **Groq (recommended — fast):** create a free key at
  <https://console.groq.com/keys>
- **Google Gemini (larger context):** create a free key at
  <https://aistudio.google.com/app/apikey>

Paste it into **Settings** (gear icon, bottom-left). Done.

## Host it free on GitHub Pages

1. Push this repo to GitHub.
2. Repo **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Pick your branch and the **`/ (root)`** folder, then **Save**.
5. Wait a minute — your app is live at
   `https://<your-username>.github.io/<repo>/`.

Because it's one static file, there's nothing else to configure.

---

## Tuning Made-Up

Everything you'd want to change lives in clearly-marked constants at the top of the
`<script>` in `index.html`:

- `PRODUCT_NAME` — the app's name, used everywhere (single source of truth).
- `GROQ_MODEL` / `GEMINI_MODEL` — swap in any current chat model.
- `MODES` — the pluggable modes array. `app` is live; `image` is a filled-in
  example and `video` / `script` are stubs. Adding a real mode = append an object
  (or flip `enabled:true`); the engine is mode-agnostic, so nothing else changes.
- `APP_SYSTEM_PROMPT` — **the brain** for app mode: a confidence-gated interview
  that plays your idea back to you before it writes the brief. Tune it freely.
- `SENTINEL` — the marker a mode emits when it's done and the output begins.

## Privacy

- No backend, no analytics, no accounts.
- Your API key, your name, and your saved briefs live only in your browser.
- Model requests go straight from your browser to Groq or Google — nowhere else.
- **Clear all local data** any time from Settings.
