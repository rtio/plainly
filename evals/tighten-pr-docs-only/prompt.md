---
name: tighten-pr-docs-only
tags: [tighten, pr]
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
---

Clean up this PR description. Keep it short and neutral, nothing extra.

Repo PR template:

```
### Changes proposed in this Pull Request

<!-- Describe the change and why it is needed. -->

Closes #

### How to test the changes in this Pull Request

1.

### Changelog entry

- [ ] This change needs a changelog entry.
<!-- changelog-type: fix | add | update | dev -->
```

Current description:

```
### Changes proposed in this Pull Request

<!-- Describe the change and why it is needed. -->

This PR updates the README to significantly improve clarity and provide a better onboarding experience for new contributors. As part of our ongoing effort to make the project more welcoming, I took a careful pass over the documentation.

Here's a summary of what changed:
- **Getting started:** Rewrote the section so the setup steps appear in the order people actually run them.
- **Links:** Fixed three broken links to the developer docs that were returning 404.
- **Requirements:** Replaced the outdated Node 16 requirement with Node 20, which matches `.nvmrc`.

I validated every link with markdown-link-check and all 47 links resolved successfully. No code was changed, so there is no risk to existing functionality.

Closes #88

### How to test the changes in this Pull Request

1. Run `npm run lint:md` and confirm there are no warnings.
2. Open the rendered README on GitHub.
3. Click each of the three fixed links and confirm they load.
4. Confirm the Node version matches `.nvmrc`.

### Changelog entry

- [ ] This change needs a changelog entry.
<!-- changelog-type: fix | add | update | dev -->
```
