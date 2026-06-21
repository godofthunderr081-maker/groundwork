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

## Optional: sync your key across devices

By default the app is fully backend-free — your key lives only in this browser,
so clearing the browser (or switching device) means pasting it again. If you'd
rather the key **follow you across browsers and devices**, turn on cloud sync.
It's powered by [Supabase](https://supabase.com) (free tier) and is **off until
you add two values** — nothing changes until you do.

1. Create a free project at <https://supabase.com>.
2. In the project's **SQL Editor**, run:

   ```sql
   create table if not exists public.user_keys (
     user_id uuid not null references auth.users(id) on delete cascade,
     provider text not null,
     api_key text,
     updated_at timestamptz default now(),
     primary key (user_id, provider)
   );
   alter table public.user_keys enable row level security;
   create policy "own keys" on public.user_keys
     for all using (auth.uid() = user_id) with check (auth.uid() = user_id);
   ```

3. In **Project Settings → API**, copy the **Project URL** and the **anon/public**
   key, and paste them into the constants at the top of the `<script>` in
   `index.html`:

   ```js
   const SUPABASE_URL = "https://YOUR-PROJECT.supabase.co";
   const SUPABASE_ANON_KEY = "YOUR-ANON-KEY";
   ```

4. (Email is enabled by default; for production add your own SMTP under
   **Authentication → Providers → Email** so codes always send.)

Now a **Sync across devices** section appears in Settings: the user enters their
email, gets a 6-digit code, and signs in. Their key is saved to their account
and restored automatically the next time they sign in anywhere — even on a
freshly-cleared browser. The publishable anon key is safe to ship, and a
row-level-security policy means each person can only read their own key.

## Privacy

- Backend-free by default — no analytics, no accounts.
- Your API key, your name, and your saved briefs live only in your browser.
- Model requests go straight from your browser to Groq or Google — nowhere else.
- **Clear all local data** any time from Settings.
- Cloud sync (above) is **opt-in**: only if you add Supabase keys, and even then
  the only thing stored in your project is the API key, guarded so each signed-in
  user can read only their own.
