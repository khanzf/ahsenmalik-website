# Ahsen Malik for Congress — website

A static campaign website for **Ahsen Malik for Congress** (Virginia's 10th
District), built with [Hugo](https://gohugo.io/). Everything lives in this
repository, so you can host it anywhere that serves static files (GitHub
Pages, Netlify, Cloudflare Pages, an S3 bucket, etc.) and move it later
without rewriting anything.

No database, no server, no build dependencies beyond Hugo itself.

---

## Table of contents

1. [What you need](#1-what-you-need)
2. [Run the site locally](#2-run-the-site-locally)
3. [The 5-minute mental model](#3-the-5-minute-mental-model)
4. [Updating the front page](#4-updating-the-front-page)
5. [Editing the other pages](#5-editing-the-other-pages)
6. [Adding a new Issues page](#6-adding-a-new-issues-page)
7. [Changing the menu, footer, and disclaimer](#7-changing-the-menu-footer-and-disclaimer)
8. [Building for production](#8-building-for-production)
9. [Hosting on GitHub Pages](#9-hosting-on-github-pages)
10. [Folder reference](#10-folder-reference)
11. [Known gaps to close before launch](#11-known-gaps-to-close-before-launch)

---

## 1. What you need

- **Hugo (extended is fine, not required for this site).** Install it once:
  - macOS: `brew install hugo`
  - Windows: `winget install Hugo.Hugo.Extended`
  - Linux: `sudo apt install hugo` (or grab a release binary from the Hugo site)
- That's it. You don't need Node, Ruby, or anything else.

Check it's working:

```bash
hugo version
```

---

## 2. Run the site locally

From the project folder:

```bash
hugo server
```

Then open **http://localhost:1313/** in your browser. The site rebuilds and
refreshes automatically every time you save a file, so you can edit and watch the
result live. Press `Ctrl+C` to stop.

---

## 3. The 5-minute mental model

Three kinds of files matter to you:

| You want to change…                | Edit a file in… |
| ---------------------------------- | --------------- |
| **Words on a page**                | `content/`      |
| **Menu, footer, disclaimer**       | `hugo.toml`     |
| **Look / layout / colors**         | `layouts/` and `static/css/style.css` |

Most updates are just editing text in `content/` — you'll rarely touch anything
else.

Each file in `content/` starts with a block fenced by `---`. That block is called
**front matter** and holds settings (like the page title). The text *below* the
closing `---` is the page body, written in **Markdown** (plain text with simple
formatting: `## Heading`, `**bold**`, `- bullet`, `[link](/about/)`). Raw HTML
is also allowed in the body (used for the video embed, the campaign banner, and
the Contact/Get Involved forms).

---

## 4. Updating the front page

Open `content/_index.md`. The **`pillars`** front matter list drives the
three focus cards below the hero carousel (Issues / Get Involved / Donate) —
add, remove, or relink them freely. The paragraph below the closing `---` is
the intro copy above the "Congressional District 10" panel.

The three hero carousel slides (campaign video + "Right for Virginia's
Tenth" banner, About Ahsen teaser, Issues teaser) and the purple
"Congressional District 10" panel are hand-built in `layouts/index.html`
because they mix video, images, and links — edit that file directly to
change their text or targets.

---

## 5. Editing the other pages

Each sub-page is one Markdown file:

| Page             | File                          |
| ---------------- | ------------------------------ |
| About Ahsen      | `content/about.md`             |
| Issues (hub)     | `content/issues/_index.md`     |
| Get Involved     | `content/get-involved.md`      |
| Contact          | `content/contact.md`           |
| Donate           | `content/donate.md`            |

Open the file, edit the text under the front matter, and save. Markdown quick
reference:

```markdown
## A section heading

A normal paragraph with **bold** and a [link to another page](/contact/).

- a bullet
- another bullet

> A highlighted callout box.

| Column A | Column B |
| --- | --- |
| Cell | Cell |
```

---

## 6. Adding a new Issues page

Issue positions live in `content/issues/` and appear both on the `/issues/`
hub page and as a dropdown under **Issues** in the top nav. To add one:

1. Create `content/issues/your-issue.md` with `title` and `description`
   front matter, then the position below the `---`.
2. Add a matching entry to `[menus]` in `hugo.toml` with
   `parent = "Issues"` (copy an existing issue entry and adjust `name`,
   `pageRef`, and `weight`).

The `/issues/` hub page lists every page under `content/issues/`
automatically — no extra step needed there.

---

## 7. Changing the menu, footer, and disclaimer

All of this is in **`hugo.toml`**.

**Menu** — under `[menus]`. Each entry is a link in the top nav. Lower
`weight` appears first. Entries with `parent = "Issues"` render as a
dropdown under the Issues link instead of their own top-level item.

**Footer details and the legal disclaimer** — under `[params]`:

```toml
[params]
  description = "…tagline shown under the footer heading…"
  disclaimer = "Paid for by Ahsen for Virginia."
```

> **Important:** Federal candidate committees are required to include a
> "Paid for by…" disclaimer on campaign materials, including websites. The
> `disclaimer` value is printed in the footer of every page. If the
> committee's registered name changes, update this one line and it changes
> everywhere.

---

## 8. Building for production

When you're ready to publish, generate the finished static files:

```bash
hugo
```

Everything is written to a `public/` folder. Those are plain HTML/CSS files you
can upload to any host. (You normally don't commit `public/` — see the included
`.gitignore`.)

---

## 9. Hosting on GitHub Pages

The portable, free option:

1. Create a repository on GitHub and push this project to it.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **GitHub Actions**.
4. Pick the **Hugo** workflow GitHub suggests and commit it.

From then on, every `git push` rebuilds and redeploys the site automatically.
Because the whole project is in the repo, you can switch to Netlify, Cloudflare
Pages, or your own server at any time by pointing them at the same repository and
running `hugo`.

Set your real domain in `hugo.toml`:

```toml
baseURL = "https://www.ahsenforvirginia.com/"
```

---

## 10. Folder reference

```
ahsenmalik-website/
├── hugo.toml               ← site settings: menu, footer, disclaimer
├── README.md                ← this file
├── archetypes/
│   └── default.md           ← template used by `hugo new`
├── content/                 ← YOUR WORDS LIVE HERE
│   ├── _index.md             ← the front page (pillars + intro copy)
│   ├── about.md
│   ├── get-involved.md
│   ├── contact.md
│   ├── donate.md
│   └── issues/
│       ├── _index.md         ← the Issues hub page
│       ├── abortion.md
│       ├── climate.md
│       ├── data-centers.md
│       ├── inflation.md
│       ├── iran-and-israel.md
│       ├── health-security.md
│       └── election-reform.md
├── layouts/                  ← HTML templates (rarely need editing)
│   ├── index.html            ← homepage layout (carousel + pillars + district panel)
│   ├── 404.html
│   ├── _default/
│   │   ├── baseof.html       ← page shell (header + footer wrapper)
│   │   ├── single.html       ← layout for sub-pages
│   │   └── list.html         ← layout for the Issues hub
│   └── partials/
│       ├── head.html
│       ├── header.html       ← logo + top navigation (incl. Issues dropdown)
│       └── footer.html       ← footer + required disclaimer
└── static/
    └── css/
        └── style.css         ← all colors, fonts, and layout
```

---

## 11. Known gaps to close before launch

This template implements the content and structure requested, but a few
pieces depend on accounts or services only the campaign can set up:

- **Contact form** (`content/contact.md`) has no backend yet — submitting it
  currently just reloads the page. Wire it to a form service (Formspree,
  Netlify Forms, a Google Form, etc.) or a custom endpoint before launch.
- **Get Involved** embeds the Google Form exactly as provided. Renaming
  "Volunteer Summer Fellowship" to "Volunteer," allowing multi-select,
  removing the car/laptop/start-date questions, and adding the resume note
  all need to be done inside the Google Forms editor — that's outside this
  repository.
- **Donate** (`content/donate.md`) is intentionally left as a placeholder for
  the campaign's donation processor.
- The homepage's "Right for Virginia's Tenth" banner and the "Congressional
  District 10" panel are built with CSS/HTML rather than the original
  graphic files, since those image files weren't available in this
  repository — swap in real brand assets any time by editing
  `layouts/index.html` and `static/css/style.css`.
