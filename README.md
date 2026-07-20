# NoVA Turnout — website

A static website for the **NoVA Turnout** political action committee, built with
[Hugo](https://gohugo.io/). Everything lives in this repository, so you can host
it anywhere that serves static files (GitHub Pages, Netlify, Cloudflare Pages, an
S3 bucket, etc.) and move it later without rewriting anything.

No database, no server, no build dependencies beyond Hugo itself.

---

## Table of contents

1. [What you need](#1-what-you-need)
2. [Run the site locally](#2-run-the-site-locally)
3. [The 5-minute mental model](#3-the-5-minute-mental-model)
4. [Updating the front page](#4-updating-the-front-page)
5. [Editing the other pages](#5-editing-the-other-pages)
6. [Adding a brand-new page](#6-adding-a-brand-new-page)
7. [Changing the menu, footer, and disclaimer](#7-changing-the-menu-footer-and-disclaimer)
8. [Building for production](#8-building-for-production)
9. [Hosting on GitHub Pages](#9-hosting-on-github-pages)
10. [Folder reference](#10-folder-reference)

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
| **Menu, footer, email, disclaimer**| `hugo.toml`     |
| **Look / layout / colors**         | `layouts/` and `static/css/style.css` |

Most updates are just editing text in `content/` — you'll rarely touch anything
else.

Each file in `content/` starts with a block fenced by `---`. That block is called
**front matter** and holds settings (like the page title). The text *below* the
closing `---` is the page body, written in **Markdown** (plain text with simple
formatting: `## Heading`, `**bold**`, `- bullet`, `[link](/about/)`).

---

## 4. Updating the front page

The homepage is special: it's driven almost entirely by front matter so you can
change the headline and stats without touching any HTML. Open:

```
content/_index.md
```

You'll find clearly labelled fields. The ones you'll touch most:

```yaml
headline: "Every election is decided by who shows up."
subhead:  "NoVA Turnout helps neighbors..."

# The turnout meter — update these two numbers after each push:
meter_label:   "Pledged voters this cycle"
meter_current: 6200
meter_goal:    10000
```

- **`meter_current` / `meter_goal`** drive the gold progress bar. The percentage
  fills in automatically. After a registration push, bump `meter_current`.
- **`pillars`** is the list of three cards ("Register / Plan / Turn out"). Add or
  remove items and the layout adjusts.
- The paragraphs **below** the closing `---` are the intro copy at the bottom of
  the page. Edit them like any document.

Save, and the local preview updates instantly.

---

## 5. Editing the other pages

Each sub-page is one Markdown file:

| Page         | File                       |
| ------------ | -------------------------- |
| About        | `content/about.md`         |
| Get Involved | `content/get-involved.md`  |
| Contact      | `content/contact.md`       |

Open the file, edit the text under the front matter, and save. Markdown quick
reference:

```markdown
## A section heading

A normal paragraph with **bold** and a [link to another page](/contact/).

- a bullet
- another bullet

> A highlighted callout box.
```

---

## 6. Adding a brand-new page

From the project folder:

```bash
hugo new content endorsements.md
```

This creates `content/endorsements.md` from the template in `archetypes/`. Edit
its text, then add it to the menu (see next section). The page will live at
`/endorsements/`.

---

## 7. Changing the menu, footer, and disclaimer

All of this is in **`hugo.toml`**.

**Menu** — under `[menus]`. Each entry is a link in the top nav. Lower `weight`
appears first:

```toml
[[menus.main]]
  name = "Endorsements"
  pageRef = "/endorsements"
  weight = 35
```

**Footer details, email, social links, and the legal disclaimer** — under
`[params]`:

```toml
[params]
  email = "info@novaturnout.org"
  disclaimer = "Paid for by NoVA Turnout. Not authorized by a candidate or candidate's committee."
  twitter = ""   # leave blank to hide the link
```

> **Important:** Virginia law requires a "Paid for by…" disclaimer on PAC
> materials, including websites. The `disclaimer` value is printed in the footer
> of every page. If your committee becomes candidate-authorized or changes its
> registered name, update this one line and it changes everywhere.

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
baseURL = "https://www.novaturnout.org/"
```

---

## 10. Folder reference

```
nova-turnout/
├── hugo.toml              ← site settings: menu, footer, disclaimer, email
├── README.md              ← this file
├── archetypes/
│   └── default.md         ← template used by `hugo new`
├── content/               ← YOUR WORDS LIVE HERE
│   ├── _index.md          ← the front page (hero, meter, pillars, intro)
│   ├── about.md
│   ├── get-involved.md
│   └── contact.md
├── layouts/               ← HTML templates (rarely need editing)
│   ├── index.html         ← homepage layout
│   ├── _default/
│   │   ├── baseof.html    ← page shell (header + footer wrapper)
│   │   ├── single.html    ← layout for sub-pages
│   │   └── list.html
│   └── partials/
│       ├── head.html
│       ├── header.html    ← logo + top navigation
│       └── footer.html    ← footer + required disclaimer
└── static/
    └── css/
        └── style.css      ← all colors, fonts, and layout
```

---

### A note on campaign-finance compliance

This site ships with the required Virginia PAC disclaimer in the footer and a
contributor-information note on the *Get Involved* page. It is a starting point,
not legal advice — confirm your specific disclaimer wording and reporting
obligations with your treasurer or the Virginia Department of Elections.
