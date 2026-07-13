---
layout: post
title:  "Jekyll Windows Install"
date:   2026-07-12 20:14:00 -0500
categories: jekyll install
---
Download/Install [Ruby+Devkit][jekyll-download]

| Action | Command |
| ------ | ------- |
| Check RubyGems package manager version | `gem -v` |
| Update RubyGems package manager | `gem update --system` |
| Install jekyll | `gem install jekyll` |
| Check jekyll version | `jekyll -v` |
| Update jekyll | `gem update jekyll` |
| Create new site | `jekyll new <site-name>` |
| Launch local server | `bundle exec jekyll serve` |

Silence deprecation warnings, edit _config.yml:
- replace `platforms :mingw, :x64_mingw, :mswin, :jruby` with `platforms :windows, :jruby`
- replace `:platforms => [:mingw, :x64_mingw, :mswin]` with `:platforms => :windows`
- add sass block:

```
sass:
  quiet_deps: true
  silence_deprecations:
    - import
```

- restart local server: `bundle exec jekyll serve`

[jekyll-download]: https://rubyinstaller.org/downloads/
