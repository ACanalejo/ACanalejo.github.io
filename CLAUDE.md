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
- Research areas: comparative politics, electoral behavior, affective polarization,
  politics of digitalization, Spanish regional identities

---

## Technology Stack

- **Generator:** Jekyll (static site)
- **Theme:** [al-folio](https://github.com/alshedivat/al-folio)
- **Hosting:** GitHub Pages at https://acanalejo.github.io
- **Ruby gems:** See `Gemfile` — key ones are `jekyll-scholar` (bibliography),
  `jekyll-paginate-v2` (blog), `jekyll-archives` (blog archives)
- **Build:** `bundle exec jekyll serve` locally; GitHub Actions deploys on push to `main`

---

## File Structure and Content Flow

**Verify this section against the actual repo before acting on it** — pages may have been
added, removed, or restructured since this was written.

### Pages (`_pages/`) — current state

| File | Live URL | Content source |
|---|---|---|
| `about.md` | `/` (Home) | Inline markdown — single integrated bio in a few paragraphs (no subsections/headings), heavily hyperlinked including inline links out to specific Publications/Work in Progress entries |
| `publications.md` | `/publications/` | jekyll-scholar `{% bibliography %}` tags pulling from `_bibliography/papers.bib` |
| `research.md` | `/research/` | Inline markdown, titled "Work in Progress" — working papers and ongoing (unpublished) projects only |
| `research-grants.md` | `/research-grants/` | Inline markdown — grants/projects as PI vs. as non-PI |
| `teaching.md` | `/teaching/` | Inline markdown — course listings with GitHub links, supervision counts, and a "Teaching Resources" section embedding the ShinyApps iframes (moved from the former `software.md`) |
| `outreach.md` | `/outreach/` | Inline markdown — reverse-chronological media appearances |
| `cv.md` | `/cv/` | Front matter only (`cv_pdf:` URL); rendered by `_layouts/cv.liquid` from `_data/cv.yml` — shows an inline PDF embed plus a download button |
| `dropdown.md` | (navbar) | Front matter only — defines the navbar dropdown and its child links |
| `blog.md` | `/blog/` | Layout wrapper — lists `_posts/`; currently empty (no posts exist) |
| `404.md` | `/404.html` | Static error page |

Navbar order: Publications, Work in Progress, Research Grants, Outreach, Teaching, CV.
There is no standalone Software page — its two ShinyApps iframes now live under Teaching.

If a page has been added or removed since this was written, update this table.

### Data files (`_data/`)

| File | Purpose |
|---|---|
| `socials.yml` | Social profile links rendered in the footer/about page |
| `cv.yml` | CV page content — a single PDF entry pointing to the locally hosted `assets/pdf/long_cv_canalejo.pdf` (manually synced from the Quarto CV project; GitHub raw URLs force a download instead of rendering inline, so keep this local) |
| `venues.yml` | Background colors for journal-abbreviation badges on the Publications page (`_layouts/bib.liquid` reads `site.data.venues[entry.abbr].color`) — add an entry here whenever a new journal abbreviation is used in `_bibliography/papers.bib`, or its badge renders invisible (white text, no background) |
| `outlets.yml` | Background colors for the outlet tag badges on `_pages/outreach.md` — add an entry whenever a new outlet is added |

### Key `_config.yml` settings to know

- `scholar: last_name / first_name` — author name for bibliography highlighting; should
  match the site owner's actual name
- `announcements: enabled` — controls whether `_news/` items appear on the home page
- `latest_posts: enabled` — controls whether recent blog posts appear on the home page
- `bib_search: true` — bibliography search on the Publications page, now enabled
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
