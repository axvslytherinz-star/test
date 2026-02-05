## What's New (2026-02-05)

- **Modernized Login System:**
  - Login modal now features two clear tabs: User (Ignito) and Admin.
  - User login: enter password (austin, xavian, vaughn) and (optionally) a custom ignito ID (random if blank).
  - Admin login: requires Admin ID (`666000999`), Username (`axvadmin609`), and Password (`elite609`).
  - All fields are validated; login is not possible without correct credentials.
  - Responsive, visually balanced, and mobile-friendly login UI.
  - Profile dropdown in navbar shows user/admin info and a red "Exit" button for logout/lockout.
  - Session logic is robust; login modal always appears if not authenticated.

- **Security Reminder:**
  - All authentication is client-side only (sessionStorage). This is for privacy/convenience, not for true security.

- **UI/UX Improvements:**
  - Login modal and profile UI are now visually modern, accent-colored, and responsive.
  - Admin and user flows are clearly separated and easy to use.

AXV Elite — Site Architecture

Overview
- Minimal static site (HTML / CSS / Vanilla JS).
- Purpose: semi-private academic archive and structured notes.
- No frameworks, no back end, no build tools.

Structure
/
├─ index.html
├─ research.html
├─ blog.html
├─ article.html          (Listing / hero for article cards)
├─ about.html
├─ contact.html
├─ 404.html
├─ articles/             (Rendered article pages — static shells)
│  ├─ ai-and-strategic-control.html
│  └─ coordination-failure-in-supply-networks.html
├─ assets/
│  ├─ css/
│  │  ├─ main.css
│  │  ├─ theme.css
│  │  └─ brutalist.css
│  └─ js/
│     ├─ main.js
│     ├─ private.js
│     ├─ article-renderer.js
│     └─ articles/
│        ├─ index.js        (registry for listing)
│        ├─ ai-and-strategic-control.js
│        └─ coordination-failure-in-supply-networks.js

Key behaviors
- Private mode: client-side password gate (sessionStorage flag). Passwords are matched client-side.
- Article listing: `article.html` is the central listing (grid of cards). The listing reads `window.ARTICLES` (assets/js/articles/index.js).
- Per-article pages: static HTML shells under `/articles/` that load a per-article JS file (`assets/js/articles/<slug>.js`) which defines `window.ARTICLE`; `assets/js/article-renderer.js` renders it into the page.
- Header: logo on left, nav on right; header gains a subtle blur and visible white text on scroll.
- Visual theme: strict monochrome (black & white) — see `assets/css/main.css`. A `brutalist` class is available for a harsher portrait.


## Access & Authentication

### Admin Login
- Admins log in with a unique username and password (see below). This grants access to all admin features, including article editing and profile management.
- Admin credentials (for your use):
  - **Username:** axvadmin
  - **Password:** elite2026
- After successful login, admin session is set in sessionStorage (`axv_admin` = 'true').

### User Login (Ignito Mode)
- Users log in with one of the main passwords (austin, xavian, vaughn). This grants read-only access.
- After successful login, a unique ignito id is generated and stored in sessionStorage for the session.

### Profile & Logout
- The navigation bar includes a profile dropdown (shows username or ignito id, and a red "Exit" button).
- Clicking "Exit" clears session and shows a 404 error page. Returning to the site requires logging in again.

---


Security notes
- Passwords are stored in client-side code and should not be considered secure. This gate only provides a minimal access control for casual privacy; do not rely on it for true confidentiality.
- For higher security, migrate to a server-backed authentication model.

How to add an article (static, without a build step)
1. Add a JS file in `assets/js/articles/<slug>.js` that defines `window.ARTICLE = { slug, title, date, tags: [], content: '<p>html…</p>' }`.
2. Add an entry to `assets/js/articles/index.js` with metadata (slug, title, abstract, date, tags).
3. Create a static shell at `articles/<slug>.html` that includes the per-article JS file and `article-renderer.js`.

If you want, I can add a simple scaffolding script to create new article files with a template.

Maintenance notes
- Use relative paths for local file testing. Article shells under `/articles/` reference `../assets/...` and `../index.html` by design so files work when opened directly in a browser.
- Keep `assets/js/articles/index.js` as the single registry of articles. Adding an entry there will cause the listing page to render the card automatically (no build step required).
- Per-article data is canonical in `assets/js/articles/<slug>.js` set `window.ARTICLE` so `article-renderer.js` can render it.
- To add a new article, copy the file `articles/complexity-is-rarely-accidental.html` and `assets/js/articles/complexity-is-rarely-accidental.js` and update their contents and the registry.

Suggested prompt templates (improved)
- Add article
  "Add a new research article with slug: <slug>, title: <title>, date: <YYYY>, tags: [<tag>,...], abstract: '<2–3 sentence abstract>', content: '<html-formatted content>'. Please create `assets/js/articles/<slug>.js`, add it to `assets/js/articles/index.js`, and create `articles/<slug>.html` shell linked to it. Use conservative, academic tone in the content."

- Update site behavior
  "Change header behavior: make header auto-hide on scroll down and show on scroll up; keep it accessible and without animation. Keep color palette monochrome."

- Security note for prompts
  "When requesting changes related to access control, explicitly confirm you understand the site uses client-side passwords (sessionStorage) and that this method is not secure for sensitive content."

If you want a scaffolding helper (optional), tell me and I will add a small, single-file Node script that creates the JS and HTML shells for a new article (run locally, optional).
