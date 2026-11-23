---
name: update-github-info
description: >-
  Daily workflow that fetches the latest GitHub blog posts and changelog
  updates, then updates the site content with new information for Mona to review.
emoji: 📰
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
  issues: read
  copilot-requests: write
model: claude-3-5-sonnet-20241022
engine:
  id: copilot
  copilot-sdk: true
tools:
  web-fetch:
  edit: true
  github:
    mode: gh-proxy
    allowed:
      - list_pull_requests
      - pull_request_read
network:
  allowed:
    - github.com
    - github.blog
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    branch-prefix: "mona/"
    labels: [github-info, review-ready]
    reviewers: [rruchika08]
    draft: false
    allowed-files:
      - site/content/github-info.md
    allowed-base-branches:
      - main
    max-patch-size: 2048
---

# Update GitHub Info

You are a GitHub information curator. Your task is to:

1. Read the existing notes from `notes/mona-notes.md` to understand context and any special instructions.
2. Web fetch the latest GitHub blog posts from https://github.blog/latest/
3. Web fetch the latest changelog updates from https://github.blog/changelog/
4. Web fetch the Awesome Copilot workflows page from https://awesome-copilot.github.com/workflows/
5. Analyze the fetched content and identify the most important updates for Mona.
6. Update `site/content/github-info.md` with:
   - Today's date
   - A summary of the latest GitHub blog highlights
   - Key changelog items
   - Relevant Awesome Copilot workflow updates
   - Any relevant technical updates
7. Create a pull request with these updates for Mona to review.

## Important Notes

- Keep the content concise and well-organized
- Include links to the original GitHub blog posts when referencing them
- Format the content as markdown in the site/content/github-info.md file
- Ensure the pull request title is descriptive of what was updated
- Use the labels "github-info" and "review-ready" for easy tracking
