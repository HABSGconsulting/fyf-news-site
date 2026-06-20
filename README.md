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

## Multilingual Setup & Known Gotchas

Hugo's `defaultContentLanguage: en` assigns all `.md` files to the `en` site.
The `hi` site sees content only if Hugo can find a Hindi translation.

### Why `site.Sites` is required in templates

In a multilingual Hugo build, when a template renders for the `hi` language site,
`site.RegularPages` returns the `hi` site's own pages — which may be empty if no
separate Hindi `.md` files exist. To always reach the unified `en` page pool:

```go
{{- $allPages := where (index site.Sites 0).RegularPages "Section" "news" }}
```

`site.Sites` returns all language sites in the build. Index `0` is always the
default language site (`en`). This is the correct pattern for cross-language
page access in Hugo multilingual builds.

**Common mistake:** `hugo.Sites` — `hugo` is the build-info object (version,
environment). It has no `.Sites` field. Always use `site.Sites`.

### GroupByDate limitation

`Pages.GroupByDate` only works on Hugo's native `Pages` type. A plain Go `slice`
(built with `slice` + `append`) does **not** have this method — Hugo silently
renders nothing. Always use manual day-grouping:

```go
{{- $sorted := sort $posts "Date" "desc" }}
{{- $currentDay := "" }}
{{- range $sorted }}
  {{- $day := .Date | time.Format "2006-01-02" }}
  {{- if ne $day $currentDay }}
    {{- if ne $currentDay "" }}</div>{{- end }}
    <div class="fyf-day-block">
      <div class="fyf-day-label">{{ .Date | time.Format "Monday, 2 January 2006" }}</div>
    {{- $currentDay = $day }}
  {{- end }}
  ... card HTML ...
{{- end }}
{{- if ne $currentDay "" }}</div>{{- end }}
```

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
