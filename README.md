# White Papers

A [GitHub Pages](https://pages.github.com/) site publishing technical white papers by **Avinash Peyyety**.

**Live site:** [https://avinashpeyyety.github.io/white-papers/](https://avinashpeyyety.github.io/white-papers/)

## Papers

| Title | Date |
|-------|------|
| [Optimal Local LLM Reasoning for 24×7 Transpilation Workloads](papers/optimal-local-llm-reasoning-24x7.md) | June 2026 |

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
