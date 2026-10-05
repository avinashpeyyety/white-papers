# White Papers

A [GitHub Pages](https://pages.github.com/) site publishing technical white papers by **Avinash Peyyety**.

Writing assistance: SpaceXAI.

**Live site:** [https://avinashpeyyety.github.io/white-papers/](https://avinashpeyyety.github.io/white-papers/)

## Papers

Each paper is published at a canonical URL: `https://avinashpeyyety.github.io/white-papers/<slug>/`

### Agentic AI series (`_papers/agentic/`)

| # | Title | Slug | Date |
|---|-------|------|------|
| 1 | [The IDE Is Not an Ops Computer: A Five-Level Maturity Ladder and Its Token Economics](https://avinashpeyyety.github.io/white-papers/agentic-maturity-ladder-token-economics/) | `agentic-maturity-ladder-token-economics` | October 2026 |
| 2 | [Maximize IDE Outcomes First: Token-Efficient Access Paths from the IDE to the Data Estate](https://avinashpeyyety.github.io/white-papers/maximize-ide-outcomes-access-paths/) | `maximize-ide-outcomes-access-paths` | October 2026 |
| 3 | [Nothing New to Buy: Phase 1 Agentic Delivery with an IDE Assistant, a Skills Library and an IDE Hub](https://avinashpeyyety.github.io/white-papers/phase-1-ide-assistant-skills-hub/) | `phase-1-ide-assistant-skills-hub` | October 2026 |
| 4 | [Agentic Data Quality: From Hand-Built Rules to a Data Quality Skill at Scale](https://avinashpeyyety.github.io/white-papers/agentic-data-quality-skill-at-scale/) | `agentic-data-quality-skill-at-scale` | October 2026 |
| 5 | [The Agent Stack, Read Bottom-Up: Eight Layers from Systems of Record to the Human Gate](https://avinashpeyyety.github.io/white-papers/agent-stack-cross-section/) | `agent-stack-cross-section` | October 2026 |
| 6 | [From Copilot Speed to Autonomous Savings: Why Human-Led AI Doesn't Move Capacity, and the Five Unlocks That Do](https://avinashpeyyety.github.io/white-papers/autonomy-unlock-copilot-speed-to-savings/) | `autonomy-unlock-copilot-speed-to-savings` | October 2026 |
| 7 | [From Workbench to Factory: Agent Harnesses and a Persistent Agent Platform](https://avinashpeyyety.github.io/white-papers/agent-harnesses-persistent-platform/) | `agent-harnesses-persistent-platform` | September 2026 |
| 8 | [Tiered On-Premises Intelligence Pods: Open Models Next to Enterprise Data](https://avinashpeyyety.github.io/white-papers/tiered-on-prem-intelligence-pods/) | `tiered-on-prem-intelligence-pods` | September 2026 |

### Other papers

| Title | Slug | Date |
|-------|------|------|
| [Public Tape, Not System of Record](https://avinashpeyyety.github.io/white-papers/settlement-tape-warrant-gated-identity/) | `settlement-tape-warrant-gated-identity` | September 2026 |
| [Optimal Local LLM Reasoning for 24×7 Transpilation Workloads](https://avinashpeyyety.github.io/white-papers/optimal-local-llm-reasoning-24x7/) | `optimal-local-llm-reasoning-24x7` | June 2026 |

Legacy paths under `/papers/` redirect to the canonical slug URL.

Paper bodies live in `_papers/` (Jekyll collection); the agentic series lives in `_papers/agentic/` and is grouped on the hub page via `series: agentic` front matter. Agentic drafts are AI-assisted and marked for human review. Redirect stubs live in `papers/`.

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
