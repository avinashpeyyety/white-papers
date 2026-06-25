# White Papers

A [GitHub Pages](https://pages.github.com/) site publishing technical white papers by **Avinash Peyyety**.

Writing assistance: Composer 2.5, Moonshot Kimi 2.5, Grok Build.

**Live site:** [https://avinashpeyyety.github.io/white-papers/](https://avinashpeyyety.github.io/white-papers/)

## Papers

Each paper is published at a canonical URL: `https://avinashpeyyety.github.io/white-papers/<slug>/`

| Title | Slug | Date |
|-------|------|------|
| [Optimal Local LLM Reasoning for 24×7 Transpilation Workloads](https://avinashpeyyety.github.io/white-papers/optimal-local-llm-reasoning-24x7/) | `optimal-local-llm-reasoning-24x7` | June 2026 |

Legacy paths under `/papers/` redirect to the canonical slug URL.

## Local development

Requires Ruby and Bundler:

```bash
bundle install
bundle exec jekyll serve --baseurl "/white-papers"
```

Open [http://localhost:4000/white-papers/](http://localhost:4000/white-papers/).

## Deployment

Pushes to `main` trigger the GitHub Actions workflow in `.github/workflows/pages.yml`, which builds the Jekyll site and deploys to GitHub Pages.

## License

© 2026 Avinash Peyyety. All rights reserved.
