# Repo-specific instructions for code assistants

This is a small static website project (HTML/CSS, assets). The guidance below highlights the project's structure, common patterns, and gotchas so an AI agent can make focused, low-risk changes.

- Project shape (big picture)
  - Purely static content: top-level `index.html`, global CSS in `styles/styles.css`, and an `images/` and `fonts/` directory for assets.
  - There is a small sub-site in `wwr/` containing its own `index.html` and `styles/site-plan-rafting.css`. Treat `wwr/` as a separate page set that may share assets from the root `images/` and `fonts/` folders.

- Key files and directories
  - `index.html` — main front page; contains header/nav (`#wrapper > header`), a `main` with multiple `section`s, and a contact form.
  - `styles/styles.css` — primary stylesheet for the root site.
  - `images/` — image assets (note: filenames include spaces and mixed case, e.g. `HAPPY SOBER.png`, `HAPPYSOBER.png`).
  - `fonts/` — font files (note: filename includes an ampersand: `Sephora & Hayden.ttf`).
  - `wwr/` — subfolder with its own markup and CSS; keep edits in `wwr/` scoped unless intentionally changing the global site.

- How this repo is used (developer workflows)
  - No build or package tools are present. Preview by opening `index.html` in a browser or run a simple static server:

    python -m http.server 8000

  - For live-edit preview, recommend using VS Code Live Server. No tests or linting are present.

- Project-specific conventions and gotchas
  - All links use relative paths. When moving/renaming files, update every reference (root and `wwr/`) to avoid 404s.
  - Filenames are not normalized: some use spaces and uppercase characters. Be conservative: prefer updating references instead of renaming assets unless doing a repo-wide, coordinated change.
  - There are duplicate/near-duplicate image names (e.g., `HAPPY SOBER.png` vs `HAPPYSOBER.png`) — confirm which one the page should use before deleting or consolidating.
  - `index.html` contains malformed HTML in places (broken `<img>` tag and an unexpectedly placed/closed `</footer>`). When modifying markup, validate HTML in-browser after edits.
  - The contact form posts to `deal_with_my_form.php` which is not present. Treat server-side behaviors as out-of-scope unless a PHP backend is added.

- Safe edit rules for automation
  - Preserve existing structure: don't change global wrapper IDs (`#wrapper`) or header/nav semantics unless the change is explicitly requested.
  - When editing CSS, prefer adding classes rather than editing rule blocks that may be relied upon by multiple pages (`wwr/` may share rules by filename).
  - For image updates: place new assets in `images/`, reference them with correct relative path (`images/filename.ext`) and keep original file names unless you also update all references.
  - Fix obvious markup bugs conservatively (close unclosed tags, repair malformed attributes) and re-run a quick local preview to confirm layout.

- Examples (concrete patterns seen in this codebase)
  - Navigation links: <code>&lt;a href="wwr/index.html"&gt;WWR&lt;/a&gt;</code> — maintain relative linking style.
  - Global CSS path in HTML: <code>&lt;link href="styles/styles.css" rel="stylesheet"&gt;</code> — don't change unless adding a new stylesheet intentionally.
  - Images: use `images/HAPPYSOBER.png` or `images/HAPPY SOBER.png` depending on which asset you intend to show. Double-check case and spaces on macOS and other platforms.

- PR guidance and verification
  - Make small, atomic changes. Manually open the edited page(s) in a browser or run the local server command above to smoke test.
  - Include a short description in the PR about which pages were previewed and any remaining manual checks (e.g., image consolidation, PHP backend missing).

If anything above is unclear or you'd like me to expand a section (for example, generate a short HTML/CSS fix for the malformed markup in `index.html`), tell me which area to iterate on next.
