---
name: update-github-info
description: Refresh GitHub info content using the latest GitHub Blog and Changelog updates, then open a pull request for Mona to review.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
  issues: read
engine:
  id: copilot
  copilot-sdk: true
network:
  allowed:
    - defaults
    - github
    - github.blog
    - github.com
    - awesome-copilot.github.com
tools:
  edit:
  web-fetch:
  github:
    mode: gh-proxy
    min-integrity: approved
safe-outputs:
  create-pull-request:
    title-prefix: "[github-info] "
    labels: [github-info, needs-review]
    reviewers: [mona]
    allowed-files:
      - "site/content/github-info.md"
    protected-files: request-review
    draft: false
---

# Update GitHub Info

Read `notes/mona-notes.md` first and use it as the editorial guidance for any changes.

Then:
1. Use the webfetch tool to read `https://github.blog/latest/`.
2. Use the webfetch tool to read `https://github.blog/changelog/`.
3. Use the webfetch tool to read `https://awesome-copilot.github.com/workflows/`.
4. Summarize the most relevant GitHub updates and Awesome Copilot workflows for developers in a short, practical way.
5. Update `site/content/github-info.md` with relevant references from the GitHub Blog, GitHub Changelog, and Awesome Copilot workflows.
6. Keep the content brief, clear, and aligned with Mona's notes.
7. Open a pull request with the proposed changes for Mona to review before merging.

Requirements:
- Keep the update concise and useful, and prefer actionable guidance for developers.
- Mention the source whenever a point comes from the GitHub Blog or GitHub Changelog.
- Cite Awesome Copilot workflows when recommending a workflow from that source.
- Do not write directly to `main`; use the safe-output `create-pull-request` flow.
- Do not add unrelated files or changes.
- If no relevant updates justify a change, keep the page stable and explain the reason in the PR description.
