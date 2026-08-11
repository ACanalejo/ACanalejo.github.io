# CLAUDE.md — ACanalejo.github.io

This file is the authoritative reference for any AI agent maintaining or updating this site.
Read it fully before making any changes. **Treat it as a starting point, not ground truth** —
verify current state against the actual files and live site before acting, since the site
evolves and this file may lag behind.

---

## Who This Site Is For

**Álvaro Canalejo-Molero** is a Political Scientist. This is his academic personal website:
a professional portfolio for colleagues, students, and researchers. It is not a blog or
product site. Content priorities are publications, research, teaching, and outreach.

**Before any session involving biographical content (affiliation, position, institution,
address, project memberships), verify the current state by reading `_pages/about.md` and
the live site at https://acanalejo.github.io/ — do not rely on this file for those
details.** If you update biographical content anywhere, update this file too to keep it
current.

At the time this file was last updated:
- Position: Postdoctoral Researcher, University of Lucerne
- Project: SNSF-funded DIGIPOL project
- PhD: European University Institute, 2023
- Research areas: how election outcomes shape democratic attitudes and behavior, the
  politics of digitalization (via DIGIPOL), and inter-regional conflict / devolution
  preferences

---

## Technology Stack

- **Generator:** Jekyll (static site)
- **Theme:** [al-folio](https://github.com/alshedivat/al-folio)
- **Hosting:** GitHub Pages at https://acanalejo.github.io
- **Ruby gems:** See `Gemfile` — key ones are `jekyll-scholar` (bibliography),
  `jekyll-paginate-v2` (blog), `jekyll-archives` (blog archives)
- **Build:** `bundle exec jekyll serve` locally; GitHub Actions deploys on push to `main`

---

## Deployment — read this before assuming a push is "done"

`.github/workflows/deploy.yml` builds the site and pushes `_site/` to the `gh-pages`
branch (via `JamesIves/github-pages-deploy-action`), which is what GitHub Pages actually
serves from. **A green checkmark on this workflow does not mean the live site updated.**
This bit the user for an entire day of work in this repo's history — every commit deployed
"successfully" while the live site kept serving a stale build. Two real causes, both fixed,
both worth understanding so they don't recur:

1. **Missing `.nojekyll`.** GitHub Pages re-runs its own Jekyll build on whatever the
   source branch contains, unless a `.nojekyll` file tells it not to. `_config.yml` had
   `.nojekyll` listed in `keep_files` for a long time, but the file never actually existed
   in the repo, so it never made it into `_site`, so GitHub's own (silently failing)
   rebuild kept serving an old cached version regardless of how many of *our* deploys
   succeeded. Fixed by adding a root `.nojekyll` file and adding it to `_config.yml`'s
   `include:` list (Jekyll excludes dotfiles by default unless explicitly included — just
   having the file at the repo root isn't enough).
2. **`purgecss` was corrupting the CSS.** The workflow used to run a "Purge unused CSS"
   step after the Jekyll build, which reduced `main.css` from ~615KB to ~24KB in
   production — stripping Bootstrap's `row-cols-*` grid utilities and most custom rules
   (badges, card styling) along with it. This is why the live site could look badly broken
   while `localhost:4000` looked fine: the local dev server never runs this step. The step
   has been **removed**. If someone wants unused-CSS purging back, it needs a properly
   scoped/tested config first — don't re-add the blind `purgecss -c purgecss.config.js`
   invocation.

**How to actually verify a deploy landed**, in order of reliability:
1. Check the live site directly, not just localhost — `curl -sI https://acanalejo.github.io/ | grep -i last-modified` should show today's date after a deploy; if it's stale, something's wrong even if Actions is green.
2. Compare `git show origin/gh-pages:index.html` against what you expect — confirms the branch content itself, independent of whether Pages is actually serving it.
3. Open the live URL in a browser (not just curl) and visually check — CSS/JS issues like the purgecss one above are invisible to a plain HTML diff.

**Two separate GitHub Actions workflows commonly get confused:**
- `deploy.yml` — the one that matters for publishing. Check this one.
- `broken-links.yml` / `broken-links-site.yml` (lychee link checker) — cosmetic, unrelated
  to publishing, and **expected to show pre-existing failures**: dead links to old al-folio
  template docs (`FAQ.md`, `INSTALL.md`, `CUSTOMIZE.md`) and files that don't exist because
  dormant features were intentionally removed (`_posts`, `_news`, `_pages/projects.md`,
  `_data/coauthors.yml`, `_data/repositories.yml`, `assets/json/resume.json`). A red X here
  is not a deployment problem — don't chase it unless asked to clean up link-checker noise
  specifically.

---

## File Structure and Content Flow

**Verify this section against the actual repo before acting on it** — pages may have been
added, removed, or restructured since this was written.

### Pages (`_pages/`) — current state

| File | Live URL | Content source |
|---|---|---|
| `about.md` | `/` (Home) | Inline markdown — single integrated bio in a few short paragraphs (no subsections/headings). **Deliberate exception to the "heavy hyperlinks" style rule below**: the bio body is link-free except the "Contact me" mailto link — the user explicitly asked for links to be stripped out here, so don't "fix" it by re-adding them. The affiliation line in the page's `subtitle:` front matter keeps its own link. |
| `publications.md` | `/publications/` | jekyll-scholar `{% bibliography %}` tags pulling from `_bibliography/papers.bib`, split into "Peer-Reviewed Publications" and "Other Publications" sections. **Must be wrapped in `<div class="publications" markdown="1">...</div>`** — see the Design System section below, this isn't optional |
| `research.md` | `/research/` | Inline markdown, titled **"Ongoing Work"** (not "Work in Progress" — renamed) — "Working Papers" (status shown as a small line below each citation: under review / preprint / available on request) and "In Preparation" sections only. No "Shelved" section currently (removed by request; re-add only if asked) |
| `research-grants.md` | `/research-grants/` | Inline markdown — "As (Co-)Principal Investigator" and "As Non-PI" sections. PI entries use a custom two-line block (title/amount, then a smaller description line below) rather than plain bullets |
| `teaching.md` | `/teaching/` | Inline markdown. Order: "Courses" (intro text, then a `.projects`-wrapped card grid — one card per course via the `_projects` collection, `category: courses`), "Interactive Materials" (renamed from "Teaching Resources" — the two ShinyApps, also `.projects`-wrapped cards, `category: teaching`), then "Supervision" last |
| `outreach.md` | `/outreach/` | Inline markdown — reverse-chronological media appearances, each with a `.badge-tinted` outlet tag |
| `cv.md` | `/cv/` | Front matter only (`cv_pdf:` URL); rendered by `_layouts/cv.liquid` from `_data/cv.yml` — shows a Download button plus an inline PDF embed, no duplicate heading |
| `dropdown.md` | (navbar) | Front matter only — defines the navbar dropdown and its child links |
| `blog.md` | `/blog/` | Layout wrapper — lists `_posts/`; currently empty (no posts exist) |
| `404.md` | `/404.html` | Static error page |

Navbar order: Publications, Ongoing Work, Research Grants, Outreach, Teaching, CV.
There is no standalone Software page — its two ShinyApps apps now live under Teaching as
"Interactive Materials" cards (in the `_projects/` collection, not hardcoded iframes).

If a page has been added or removed since this was written, update this table.

### Data files (`_data/`)

| File | Purpose |
|---|---|
| `socials.yml` | Social profile links rendered in the footer/about page |
| `cv.yml` | CV page content — a single PDF entry pointing to the locally hosted `assets/pdf/long_cv_canalejo.pdf` (manually synced from the Quarto CV project; GitHub raw URLs force a download instead of rendering inline, so keep this local) |

There used to be `venues.yml` / `outlets.yml` / `course_levels.yml` for per-entry badge
colors — **deleted**. All badges (journal abbreviations, outlet tags, course levels) now
share one look via the `.badge-tinted` CSS class instead of per-entry hex colors. Don't
recreate these files; if a badge needs to stand out, that's a CSS/design decision, not a
data-file one — see Design System below.

### Key `_config.yml` settings to know

- `scholar: last_name / first_name` — author name for bibliography highlighting; should
  match the site owner's actual name
- `announcements: enabled` — controls whether `_news/` items appear on the home page
- `latest_posts: enabled` — controls whether recent blog posts appear on the home page
- `bib_search: true` — bibliography search on the Publications page, now enabled
- `enable_tooltips: true` — auto-generated heading permalink tooltips, now enabled
- `enable_navbar_social` — **leave `false`**. Looks like it adds social icons to the
  navbar; it actually *replaces* the name-brand text with icons, and only on the home
  page (`_includes/header.liquid` line ~23). Not a genuine addition, don't enable it
  expecting icons to appear alongside the name.
- `third_party_libraries.google_fonts.url.fonts` — loads `Source Serif 4` (headings) and
  `Inter` (body). These are actually wired up via CSS now (see Design System) — previously
  this loaded Roboto/Roboto Slab but no CSS anywhere referenced them, so don't assume a
  font listed here is actually in use; always check `_sass/_base.scss` for the real
  `font-family` rule
- `include: ["_pages", ".nojekyll"]` — **do not remove `.nojekyll` from this list.** See
  Deployment below; without it the site silently fails to update in production
- `display_tags` / `display_categories` — blog taxonomy labels; populate when starting a blog

### Assets (`assets/`)

- `assets/img/prof_pic.jpg` — main headshot (used in `about.md`)
- `assets/img/prof_pic_color.png` — color variant (available, not currently in use)
- `assets/pdf/long_cv_canalejo.pdf` — the CV, hosted locally so it renders inline instead of downloading; source of truth is still the separate Quarto CV project, manually synced here for now (a future session may migrate the Quarto build into this repo via CI — out of scope until then)

### Plugins (`_plugins/`)

Custom Ruby plugins that ship with al-folio. Do not modify or delete:
`cache-bust.rb`, `details.rb`, `download-3rd-party.rb`, `file-exists.rb`,
`google-scholar-citations.rb`, `hide-custom-bibtex.rb`, `inspirehep-citations.rb`,
`remove-accents.rb`

---

## Dormant Infrastructure — Kept Intentionally

The following exists in the repo but is currently inactive. **Do not remove it.**
It was kept deliberately to make future features easy to activate.

### Blog

To add a blog:
1. Create `_posts/YYYY-MM-DD-title.md` with standard Jekyll front matter
2. Optionally set `latest_posts: enabled: true` to surface recent posts on the home page
3. Add a blog link to `_pages/dropdown.md` if desired

Pre-wired: `jekyll-paginate-v2`, `jekyll-archives`, `classifier-reborn` are installed;
pagination and related-posts config is in `_config.yml`; `_pages/blog.md` exists as the
listing page; `giscus:` config block is present (needs `repo_id` and `category_id` to
activate comments).

### Announcements / News

To surface news items (new paper, upcoming talk, etc.):
1. Create `_news/` and add `.md` files using `layout: post`
2. Set `announcements: enabled: true` in `_config.yml`

The `news` collection is already defined in `_config.yml`.

---

## Design System

The site got a visual pass this session (typography, badge colors, card polish). Key
pieces, so future changes stay consistent instead of drifting back to al-folio defaults:

- **Fonts:** `Source Serif 4` for headings (`h1, h2, h3, .post-title, .card-title,
  h2.bibliography, .navbar-brand.title`), `Inter` for body text — rules live in
  `_sass/_base.scss` near the top. Heading sizes were deliberately tuned down from
  Bootstrap defaults after the serif swap read as too large (`.post-title: 1.9rem`,
  `h2: 1.5rem`, `h3`/`.card-title: 1.15rem`, `.navbar-brand.title: 1.05rem` — the last one
  specifically to stop the navbar name wrapping the nav links onto two lines).
- **Badges:** one shared `.badge-tinted` class (tinted background + accent border/text,
  using `--global-theme-color` / `--global-theme-tint`) for journal abbreviations
  (Publications), outlet tags (Outreach), and course levels (Teaching). `--global-theme-tint`
  is defined per-theme (light/dark) in `_sass/_themes.scss`, same pattern as every other
  color token there — add new tokens there, not as one-off inline styles.
- **The `.publications` / `.projects` wrapper-class gotcha:** al-folio's bibliography and
  project-card CSS (list-marker removal, badge spacing, card hover, heading sizing) is
  scoped under `.publications { ... }` and `.projects { ... }` respectively in
  `_sass/_base.scss`. These classes only get added automatically by layouts we don't use
  (`related_publications` for citing papers in a blog post; a full projects listing page).
  **Any custom page that renders bibliography entries or project cards must manually wrap
  that content in `<div class="publications" markdown="1">...</div>` or `<div
  class="projects">...</div>`.** Forgetting this is exactly what made Publications and
  Teaching look "flat" and broken (invisible badges, browser-default numbered list, no
  card spacing) for most of this session before it was diagnosed — the CSS was never the
  problem, the missing wrapper class was.
- **Card hover:** `.projects .card.hoverable:hover` gets a lift + shadow. Reuse this for
  any future clickable card rather than inventing a new hover treatment.
- **Never italicize text inside a hyperlink** — see the Emphasis bullet below, same root
  cause class of bug as the wrapper-class issue: a global rule silently overriding
  something that looks fine in isolation.
- If proposing visual changes again, the pattern that worked well: build a static HTML
  comparison artifact (not live files) showing 2-3 concrete options per area, grounded in
  real site content rather than lorem ipsum, and let the user pick before touching any repo
  file. See this session's `design-options.html` approach if useful as a template.

---

## Writing Style and Tone

Match this style when drafting or editing content. If existing pages have drifted from
this description, trust the pages over this file.

- **Register:** Academic but conversational — formal enough for a professional audience,
  warm enough to feel human
- **Person:** First person singular throughout
- **Links:** Heavy use of hyperlinks — institutions, projects, papers, co-authors, and
  external resources are nearly always linked inline
- **Structure:** Long pages use `####` subheadings, `---` dividers, and explicit
  `<div style="margin-top: Xpx;"></div>` spacers for visual rhythm
- **Emphasis:** `**bold**` for key research concepts; italics used rarely. **Never italicize text inside a hyperlink** — `_sass/_base.scss` sets `em { color: var(--global-text-color) }` globally, which overrides the link color and makes the text render as invisible-looking black instead of a blue link (found when linking journal names in `about.md`; fixed by dropping the italics, not by patching the CSS)
- **Citations:** Full inline citations with DOIs, award callouts inline
  (e.g., "Winner of the Wilson Award..."), not abbreviated
- **Tone on research:** Precise about findings and scope — specific empirical claims,
  not vague descriptors
- **Language:** English primary; outreach entries may be in Spanish or French — do not
  translate them
- **Personal touches:** Occasional warm asides ("Thanks for visiting!", references to
  personal background) that soften the professional tone — preserve these

---

## Standing Workflow Rules

These rules apply to every session. Follow them without being asked.

### Before committing

1. Run `bundle exec jekyll build` and confirm it completes without new errors
   (pre-existing Sass deprecation warnings and Windows Imagemagick path errors are
   normal baseline noise — ignore them)
2. Start `bundle exec jekyll serve --skip-initial-build` and tell the user exactly which
   pages to check at `localhost:4000`, based on what changed
3. **Wait for explicit confirmation before running `git commit`** — "looks good",
   "go ahead", "commit it", or similar
4. **Never push without a separate explicit push instruction** — approval to commit is
   not approval to push

### After pushing

- Don't consider the work done just because the push succeeded or the `deploy.yml` Action
  went green — see the Deployment section above. Check the actual live site (curl or
  browser) before telling the user their changes are live, especially for anything CSS/JS
  related, since a broken build can still report "Success."

### Before any deletion or structural change

1. **Commit the current working state first** as a safety checkpoint
2. Check whether what you're about to remove might be dormant-but-intentional
   infrastructure (see the Dormant Infrastructure section above)
3. **If you did not add it yourself in the current session, ask before removing it** —
   describe what it is and why you think it's safe to remove, and wait for confirmation

### Proposing new files or content

- Always propose the content first and wait for approval before writing to disk
- This applies to new pages, new data files, images, and this file itself
- You may draft and propose `.md` pages (e.g. a new publications page, a blog post),
  just don't create them without showing the content first

### Keeping this file current

When any of the following change, update this file in the same commit:
- Site owner's position, affiliation, or institution
- A new page is added or an existing page is removed
- A dormant feature is activated (blog, publications, announcements)
- A structural change is made to how content flows (new layout, new data file, etc.)
