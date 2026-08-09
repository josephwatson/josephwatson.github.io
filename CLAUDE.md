# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal Jekyll blog (`josephwatson.github.io`), deployed via GitHub Pages. It uses the stock `minima` theme (`~> 2.5`) with a small set of local overrides rather than a custom theme — most of Minima's templates/styles are inherited from the gem, and only specific files are overridden locally.

## Commands

Run everything through `bundle exec` so the pinned gem versions (see `Gemfile.lock`) are used.

```
bundle install                # install/sync gems after editing Gemfile
bundle exec jekyll serve      # local dev server at http://localhost:4000, live rebuild
bundle exec jekyll build      # one-off build to _site/
```

There is no test suite, linter, or CI configured in this repo.

## Architecture

- **Theme overriding pattern**: Jekyll/Minima resolves `_layouts/`, `_includes/`, and `assets/` locally before falling back to the `minima` gem's own copies. This repo only overrides three things:
  - `_layouts/home.html` — copy of Minima's home layout, customized to center the post list (`.home .post-list`).
  - `_includes/header.html` and `_includes/footer.html` — both **intentionally empty files**, which suppresses Minima's default site header/footer nav entirely (per commit "Hide header and footer").
  - `assets/main.scss` — imports `@import "minima";` (pulls in the full theme stylesheet) then layers on customizations: centers the home post list, and adds a `prefers-color-scheme: dark` media query with a GitHub-dark-inspired palette overriding links, code blocks, tables, headers/footers, etc. When touching dark mode styles, keep selectors scoped inside that media block and mirror Minima's class names (`.site-header`, `.site-footer`, `.svg-icon`, etc.).
- **Site config** (`_config.yml`): `theme: minima`, `plugins: [jekyll-feed]`, `baseurl: ""`, `url: "https://josephwatson.github.io"`. Sass has `quiet_deps: true` and silences the `import` deprecation warning (Jekyll 4.4's Dart Sass compiler; see the install post below for how/why this was added).
- **Content**: Posts live in `_posts/` as `YYYY-MM-DD-title.markdown` with standard Jekyll front matter (`layout: post`, `title`, `date`, `categories`). `index.markdown` uses `layout: home`; `about.markdown` uses `layout: page`.
- **Windows dev notes**: `_posts/2026-07-12-jekyll-install.markdown` documents the local Windows/Ruby+Devkit setup, including the `platforms :windows, :jruby` fix in the `Gemfile` and the `sass.quiet_deps`/`silence_deprecations` config — useful background if gem/platform issues come up again. The `Gemfile` also conditionally includes `wdm` and `tzinfo-data` for Windows.
- Code blocks in posts use the Liquid `{% highlight %}` tag rather than plain Markdown fences (per repo convention/commit history).
