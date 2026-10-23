# subjexts/publish

A GitHub Action for authors of books that plug into [subjexts](https://subjexts.github.io/). After your own
build step (`quarto render`, `jb build`, anything that produces HTML), it reads your `subjexts.yml`, scans the
built folder (pages, titles, headings, page-to-source map) and sends that **description** to the platform. Your
HTML is never copied anywhere; your repo holds no secret of ours.

```yaml
# .github/workflows/publish.yml (after your render step)
permissions:
  id-token: write      # lets the Action prove which repo it runs in
  contents: read
steps:
  - uses: actions/checkout@v4
  - run: quarto render           # your build
  - uses: subjexts/publish@v1
    with:
      manifest: subjexts.yml     # default
```

The platform checks the workflow's OIDC token: its `repository` claim must match the package's registered
`source.repo`, and a `graded` question bank must come from a private repo. Register the package on the platform
first (your handle must own it).

`subjexts.yml` (minimal):

```yaml
subjexts: 1
package: your-handle/your-book
kind: book
title: Your Book
generator: quarto                 # quarto | jupyter-book | other
site: https://you.github.io/your-book/
source:
  repo: you/your-book-src
  output: _book
```

This repository holds the built Action only (`dist/index.js`); the source is in the platform's `yrS4` repo.
MIT licence.
