# Personal webiste

Made with [jekyll](https://jekyllrb.com/) and the [type theme](https://github.com/rohanchandra/type-theme).

## Run locally

Prerequisites:
- Ruby
- Bundler (`gem install bundler`)

Install dependencies:

```bash
bundle install
```

Start the site:

```bash
bundle exec jekyll serve --host 0.0.0.0 --port 4000
```

Open in browser:
- macOS/Linux: `http://127.0.0.1:4000`
- Windows (WSL): `http://localhost:4000`

Notes:
- Jekyll auto-regenerates when files change.
- Blog posts live in `_posts/` and should be named `YYYY-MM-DD-title.md`.
