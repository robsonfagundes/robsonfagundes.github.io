robsonfagundes.github.io
========================

Personal portfolio and legacy technical blog for [Robson Fagundes](https://github.com/robsonfagundes).

The homepage presents Robson as a Software Engineer, entrepreneur, founder of Rdicom and AI Software Engineer focused on HealthTech, PACS, DICOM, cloud platforms, medical imaging workflows, APIs, integrations, databases, automation and software architecture.

The old JavaScript and web development articles are preserved as legacy technical content under the same Jekyll post structure.

Project structure
-----------------

- `index.html`: portfolio landing page content.
- `_layouts/portfolio.html`: dedicated homepage layout.
- `_layouts/post.html`: legacy blog post layout.
- `_posts/`: existing technical articles.
- `css/main.sass`: site styles, including the portfolio theme.
- `_config.yml`: GitHub Pages and site metadata.

Running locally
---------------

This site is intentionally simple and compatible with GitHub Pages/Jekyll.
Ruby 3.x is recommended for local development.

```bash
bundle install
bundle exec jekyll serve
```

Then open:

```text
http://127.0.0.1:4000/
```

Notes
-----

The site is based on the original GitHub Pages + Jekyll setup inspired by the Mediator theme:

https://github.com/dirkfabisch/mediator
