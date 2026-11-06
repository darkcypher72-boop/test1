# ELVIAN — Learn more. Work smart.

## About the developer

**Master Tonmoy Kumar Deb** is currently pursuing a Bachelor of Technology in Computer Science and Engineering. He loves turning ideas into working software, and his goal is to become a skilled software developer. He works with HTML5, CSS3, JavaScript, React, Python, C, C++ and MySQL, and he keeps adding new things to his toolbox. In August 2026 he participated in the Smart India Hackathon (SIH).

- Instagram: [@tonmoy_deb339](https://www.instagram.com/tonmoy_deb339/)
- LinkedIn: [Tonmoy Kumar Deb](https://www.linkedin.com/in/tonmoy-kumar-deb-45b4a9449/)

| Area | Skills |
|---|---|
| Programming languages | Python, C, C++, JavaScript |
| Frontend | HTML5, CSS3, JavaScript, React |
| Backend | Python |
| Frameworks & libraries | React |
| Database | MySQL |

## About Elvian

**Elvian** was made for gaining knowledge. It is a friendly assistant that helps you learn, understand and work smart: it explains things clearly, solves maths with real symbols, draws graphs and animated diagrams, shows reference pictures, reads your files and photos, and helps you finish tasks faster — so your time goes into learning and creating instead of searching.

---

## 1. Files and where they go

| File / folder | What it is | GitHub? |
|---|---|---|
| `index.html`, `e-ELVIAN.html`, `physical-elvian.html` | The website | ✅ upload |
| `manifest.webmanifest`, `sw.js`, `assets/icons/*` | Install-as-app + offline + notifications | ✅ upload |
| `assets/tonmoy.jpg` | Your photo for the About card | ✅ upload |
| `server.js`, `package.json` | Optional backend (lock code, prompt counter, AI key) | ✅ upload (contains no secrets) |
| `.env.example`, `.gitignore`, `README.md` | Template, safety list, this guide | ✅ upload |
| **`.env`** | **Your secrets** (lock-code hash, backend settings) | ❌ **NEVER** |
| `state.json` | Backend counters (auto-created) | ❌ never (ignored) |

Keep all `.html` files, `manifest.webmanifest`, `sw.js` and `assets/` in the **same folder**.

## 2. Where does `.env` go? (important)

GitHub Pages only serves **static files**. It cannot read a `.env` file and it cannot run `server.js`. So:

- **The website** (GitHub Pages) needs **no** `.env`. Everything in the browser works without it.
- **`.env` is only for the optional backend** (`server.js`): the secret lock/unlock code, the 50-prompt counter and (optionally) your AI key. The backend must run on a Node host, **separate from GitHub Pages**.
- On a host such as **Render, Railway or Fly.io** you do not upload `.env` at all. You type the same values into the host's **Environment Variables** page. That *is* the `.env` there.
- On your own PC, keep `.env` next to `server.js` and run `node server.js`.

### Set up the backend on Render (free) — step by step
1. Upload `server.js` and `package.json` to your GitHub repository (never `.env`).
2. render.com → New → **Web Service** → connect the repository. Runtime: Node. Build command: *(empty)*. Start command: `node server.js`.
3. Under **Environment**, add:
   - `UNLOCK_CODE_HASH` = the long line from your local `.env` (only the part after `=`)
   - `ALLOWED_ORIGIN` = your site address, e.g. `https://yourname.github.io` (no path, no trailing slash)
   - optional: `LLM_URL`, `LLM_KEY`, `LLM_MODEL`
4. Deploy. Copy the address Render gives you (e.g. `https://elvian-backend.onrender.com`).
5. In Elvian → **Settings → Secure backend** paste that address (or set `backend:''` near the top of `e-ELVIAN.html` and re-upload so every device uses it).
6. The free tier sleeps when idle (first request may take ~30 s) and forgets counters when it restarts. For permanent counters use a paid/persistent disk.

### If you already uploaded `.env` to GitHub
1. Delete it from the repository (`git rm --cached .env`, commit, push) and make sure `.gitignore` is uploaded.
2. Treat the code as exposed: make a **new** lock code with `node server.js --hash "new code"` and replace `UNLOCK_CODE_HASH` on your host.
3. If it held an AI key (`LLM_KEY`), **revoke it** with the AI provider and create a new one.
4. The file still exists in old commits; to remove it from history use GitHub's guide *"Removing sensitive data from a repository"* (or BFG Repo-Cleaner). Turning on **Secret scanning / push protection** (Repository → Settings → Code security) helps stop this in future.
5. The lock code is stored only as a salted scrypt hash, which is much safer than plain text — but a short or guessable code can still be cracked, so use a long one.

## 3. Install on phone / PC, notifications, widgets

**Install as an app** (needs the website to be opened from your GitHub address, not a file):
- **Android (Chrome):** ⋮ menu → *Install app* (or the **Install app** button in Elvian's sidebar).
- **iPhone / iPad (Safari):** Share → *Add to Home Screen*.
- **Windows / Mac / Chromebook (Chrome or Edge):** the install icon in the address bar.
After installing, long-pressing the app icon on Android/Windows shows shortcuts: **New chat, Ask by voice, Set a timer, Saved messages**. Elvian also appears in the Android **Share** menu so you can share text to it.

**Notifications:** Settings → *Device notifications*. Elvian then shows a notification in your device's notification panel when a **timer finishes** or an **answer is ready** while you are in another app or tab. On iPhone this works only after adding Elvian to the Home Screen (iOS 16.4+).

**Home-screen widgets — what is and isn't possible:** a website/PWA **cannot** create real Android or iPhone home-screen widgets; those must be built natively. What you get now is the installed app icon with the shortcuts above. Real widgets need a native app (Android: wrap the site with Capacitor/Trusted Web Activity and add a Kotlin widget; iOS: a Swift WidgetKit app). That is a good future project.

## 4. Lock / unlock code
- Every chat allows **50 prompts**; reminder at 40; at 50 the chat locks. **Alt+K** asks for the secret code to unlock (+50) or lock.
- The code is never in the web page; `.env` holds only a salted hash. Change it: `node server.js --hash "new code"`.
- Wrong guesses: 5 per 15 minutes per person.

## 5. Security — what is protected and what you must do
**Already built in:** all text is escaped before display; a Content-Security-Policy blocks unknown scripts, frames, plug-ins and form posts; files are read only on your device; code you run executes in an isolated background worker; links open with `noopener`; uploads are size-checked; message rate limit (6 per 30 s); the backend checks the website origin, validates every request, limits wrong codes, and never stores the code in plain text; API keys are never sent over an unprotected connection.

**You must:** never upload `.env`; keep the Google Client ID public but never a "client secret"; use a **long** lock code; if a key leaks, revoke it; prefer the backend (so the AI key stays on the server); keep your GitHub account protected with two-factor login.

**Honest limits:** a website alone cannot enforce the 50-prompt lock — anyone can edit their own browser. Real enforcement needs the backend. The CSP still allows inline scripts (the page is a single file), so it limits damage but is not a complete shield.

## 6. What Elvian can and cannot do
**Can:** chat and explain; answer today's date/time/day; maths with real symbols; graphs for any function (explicit, implicit, polar, parametric) and charts; animated diagrams; reference photos with credits and AI-made pictures; read pictures (AI vision, or text inside them), PDF, Word, PowerPoint, Excel, OpenDocument, RTF, ZIP, code/text and any other file (details at least); create PowerPoint/Word/Excel/PDF files and QR codes; timers with alarm; speak answers in a male Indian-English voice with word highlight; voice typing (stops after 3 s of silence); saved messages and saved answers; slideshow backgrounds; Google sign-in; installable app with shortcuts; device notifications.

**Cannot (yet):** make real home-screen widgets; send notifications when the app is completely closed (needs a push server); hear audio or watch video (only details and one frame); read scanned PDFs as text; guarantee live news (it uses Wikipedia/weather lookups and the AI's knowledge); enforce the prompt limit without the backend; work offline for AI answers.

## 7. Shortcuts
Enter send · Shift+Enter new line · Alt+N new chat · Ctrl+S sidebar · Ctrl+F full screen · Ctrl+H saved · Alt+D theme · Alt+S settings · Alt+M mic · Alt+L slideshow · Alt+K lock/unlock · Alt+↑ edit last · `/` focus · Esc stop · Ctrl+/ list.

## 8. Troubleshooting
- **"The free AI is busy"** — the free service is rate-limited; Elvian retries by itself. Add your own key in Settings for more speed.
- **No Indian male voice** — Windows: Settings → Time & language → Speech → add *English (India)* (Ravi / Prabhat). Edge has the best voices.
- **Google sign-in asks for a Client ID** — put it in `gid:''` at the top of `e-ELVIAN.html` and add your site address under *Authorised JavaScript origins*.
- **Photo missing in About** — add `assets/tonmoy.jpg`.
