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
3. Summarize the most relevant GitHub updates for developers in a short, practical way.
4. Update `site/content/github-info.md` with the relevant official references and product updates.
5. Keep the content brief, clear, and aligned with Mona's notes.
6. Open a pull request with the proposed changes for Mona to review before merging.

Requirements:
- Keep the update concise and useful, and prefer actionable guidance for developers.
- Mention the source whenever a point comes from the GitHub Blog or GitHub Changelog.
- Do not write directly to `main`; use the safe-output `create-pull-request` flow.
- Do not add unrelated files or changes.
- If no relevant updates justify a change, keep the page stable and explain the reason in the PR description.
