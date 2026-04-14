# CLAUDE.md — zdanowski.com

Personal site for John Zdanowski. You're helping him build and evolve it.

## Stack

- **Hugo** v0.160.1 extended + **PaperMod** theme (git submodule at `themes/PaperMod/`)
- **Vercel** auto-deploys on push to `main`
- **GitHub:** `brightzen-phaedrus/zdanowski.com`
- **Domain:** zdanowski.com (GoDaddy DNS → Vercel)
- **Live:** https://zdanowski.com
- **Vercel dashboard:** https://vercel.com/john-zdanowskis-projects/zdanowski.com

## Deploy

```bash
git add -A && git commit -m "message" && git push origin main
```

Vercel picks it up automatically. Live in ~30 seconds. No manual deploy step.

Vercel doesn't natively support Hugo — `vercel.json` downloads the Hugo extended binary at build time:

```json
{
  "buildCommand": "hugo --gc --minify",
  "outputDirectory": "public",
  "installCommand": "curl -L https://github.com/gohugoio/hugo/releases/download/v0.160.1/hugo_extended_0.160.1_Linux-64bit.tar.gz | tar xz -C /usr/local/bin",
  "framework": null
}
```

If upgrading Hugo, update the version in both vercel.json and locally.

## Local Preview

```bash
hugo server -D    # -D includes drafts
# → http://localhost:1313/
```

## Content Structure

```
content/
├── about/index.md        # About page (standalone layout)
├── posts/                 # Blog posts
│   └── hello-world.md    # Placeholder (draft: true)
└── search.md             # Search page (Fuse.js powered)
```

## Creating a Post

Create `content/posts/<slug>.md`:

```yaml
---
title: "Post Title"
date: YYYY-MM-DD
draft: false
tags: ["tag1", "tag2"]
summary: "One-line summary for list pages and search."
---
```

Body is standard markdown. Hugo auto-generates TOC and reading time.

Set `draft: true` to keep it out of production. `hugo server -D` shows drafts locally.

## Tags

Use lowercase. Suggested categories:
- `finance`, `frameworks`, `ifm`, `fourth-statement`, `weekly-accounting`
- `building-with-ai`, `phaedrus`
- `writing`, `meta`

## Site Config

Config is in `hugo.toml`. Key settings:

- **Title:** "John Zdanowski"
- **Homepage tagline:** "Better systems of thought" / "inevitably proliferate"
- **Theme:** auto (light/dark based on system preference)
- **Menu:** Posts, About, Search, Tags
- **Social:** email (z@brightzen.com), RSS
- **Features:** TOC on by default, reading time, breadcrumbs, code copy buttons, Fuse.js search

## Current Content

**About page** — sonar engineer background → financial frameworks (IFM, SEQ, Fourth Statement, Contribution Engine) → companies (PSG, BrightZen, Weekly Accounting) → books → Del Mar lifestyle → building with AI/Phaedrus. RSS subscribe link at bottom.

**Homepage** — tagline: "Better systems of thought / inevitably proliferate"

**Posts** — only a draft hello-world placeholder so far. The site is fresh.

## John's Writing Voice

Direct, first-person, grounded in physical-world metaphors (sonar, signals, noise, maps, windshields, thermostats). No corporate speak, no filler, no "unlock your potential." Short paragraphs with breathing room.

Three modes:

1. **Personal narrative** — anchored in a specific time/place/object. Opens with a concrete moment that expands into something universal. Section breaks with `~`. Ends quietly.

2. **Business/explanatory** — opens with a strong credential or analogy, uses contrast (here's how it's broken → here's the right map), weaves statistics into narrative. Physical metaphors. Lands with a quiet final statement.

3. **Philosophical/framework** — names a new concept, defines it by contrast with the status quo, uses layered examples, builds to a principle.

When in doubt, write like you're talking to a smart friend at a fire pit — not like you're writing a blog post.

## DNS (for reference)

| Type  | Host    | Value                          |
|-------|---------|--------------------------------|
| A     | @       | 76.76.21.21                    |
| CNAME | www     | cname.vercel-dns.com           |

Both apex and www are verified in Vercel. Registrar is GoDaddy.

## Background

John is a former sonar engineer turned financial framework builder. Co-founded Phoenix Strategy Group (with David Metzler), BrightZen, and Weekly Accounting (with Jeff Abrams). Created the IFM, SEQ framework, Contribution Engine, and the Fourth Statement. Writing Book 2 of *Zen and the Art of Weekly Accounting*. Lives in Del Mar, CA. Thinks on walks and at fire pits, almost never at a desk.
