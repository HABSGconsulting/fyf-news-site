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

## Deployment

Connected to Cloudflare Pages. Every push to `main` triggers an automatic build and deploy.

## Local Development

```bash
https://github.com/adityatelange/hugo-PaperMod
git clone https://github.com/HABSGconsulting/fyf-news-site
cd fyf-news-site
git submodule update --init --recursive
https://gohugo.io/
hugo server -D
```
