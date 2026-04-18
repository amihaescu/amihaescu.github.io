# andrei-mihaescu.github.io

Personal website built with Jekyll, hosted on GitHub Pages.

## Local development

```bash
bundle install
bundle exec jekyll serve
# open http://localhost:4000
```

## Writing a new article

Create a file in `_posts/` with the naming convention:

```
_posts/YYYY-MM-DD-your-article-slug.md
```

Add front matter at the top:

```yaml
---
layout: post
title: "Your Article Title"
date: 2025-03-01
category: System Design        # shown as the tag label
excerpt: "One sentence summary shown in article cards."
read_time: 5                   # estimated minutes
---

Your article content in Markdown goes here...
```

That's it. The article will appear automatically on the `/writing/` page and in the homepage preview (latest 3 posts).

## Structure

```
├── _config.yml          # site settings, author info, social links
├── _layouts/
│   ├── default.html     # wraps every page with nav + footer
│   └── post.html        # article page layout
├── _includes/
│   ├── head.html        # <head> meta, CSS link
│   ├── nav.html         # sticky navigation bar
│   └── footer.html      # site footer
├── _posts/              # articles — add yours here
├── assets/css/
│   └── main.css         # all styles
├── writing/
│   └── index.html       # /writing/ listing page
└── index.html           # homepage
```

## Deploying

Push to `main`. GitHub Pages builds and deploys automatically (usually under 60 seconds).
