Mark Turner's Blog
==================

This is the source code for Mark Turner's blog,
[Amerine.net](http://www.amerine.net).

The content of the site (blog posts and page content) is licensed under a
Creative Commons Attribution license (you may use it, but must give
attribution).

Copyright (c) 2019 Mark Turner. Rights reserved as indicated above.

## Prerequisites

* [Ruby](https://www.ruby-lang.org/) with [Bundler](https://bundler.io/) installed
  (version 2.6.7 or newer)
* [Jekyll](https://jekyllrb.com/) for building and serving the site

## Running the site locally

Install the required gems and start a development server:

```bash
bundle install
bundle exec jekyll serve
bundle exec jekyll build    # generate the site without starting a server
```

## Directory structure

* `_posts` – blog posts in Markdown
* `_projects` – project write‑ups
* `_layouts` – page and post layouts
* `_includes` – partial templates included in layouts
* `_pages` – standalone pages
* `_data` – data files used by the templates
* `_sass` – Sass partials for the site styles
* `css`, `js`, `images` – assets used by the site
