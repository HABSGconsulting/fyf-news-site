# fyf-news-site

Hugo site for [news.fundyourfreedom.in](https://news.fundyourfreedom.in)

This repo is the **output target** for the `fyf-news-engine` pipeline.
Do not edit content files manually — they are generated automatically by the pipeline.

## Structure

```
content/news/YYYY/MM/     ← impact posts (EN + HI), auto-generated
data/more-reads/          ← More Reads YAML, auto-generated
static/index/             ← chunked JSON indexes, auto-generated
static/podcast/feed.xml   ← podcast RSS, auto-generated
layouts/                  ← Hugo templates (edit here)
themes/                   ← Hugo theme
config.yaml               ← Hugo configuration
```

## Content Formats

Two post formats coexist in this repo. The template handles both transparently.

### Old format (pre June 17 2026)
- Two files per story: `slug.md` (English) + `slug.hi.md` (Hindi)
- Article body lives in the Markdown file body (below the frontmatter)
- No `content_type` param

### New format (June 17 2026+, fyf-news-engine v2)
- One file per story: `slug.md` — bilingual, all content in YAML frontmatter
- `content_type: structured` param is set
- `body_en` and `body_hi` params hold the full Markdown body for each language
- Includes additional `pro_*` fields (pro takeaway, pro questions, pro context)
- Language switch still works via Hugo's multilingual `Translations`

**Migration policy:** old posts are left as-is. They render correctly via the `.Content` fallback in `layouts/_default/single.html`. No backfill needed.

## Deployment

Connected to Cloudflare Pages. Every push to `main` triggers an automatic build and deploy.

## Local Development

Open the repo in GitHub and edit layout files directly in the browser.
For a full local preview, install [Hugo](https://gohugo.io/) and run:

```bash
git clone https://github.com/HABSGconsulting/fyf-news-site
cd fyf-news-site
git submodule update --init --recursive
hugo server -D
```
