---
name: tighten-this-pr-for-me
description: Use when the user asks to write, rewrite, shorten, or clean up a pull request description, PR comment, or commit body, especially with asks like "not overly descriptive", "not overly informative", "natural and neutral", "no slop", or "pass it through /no-ai-slop".
argument-hint: "<PR URL | PR number | draft text> (defaults to the current branch's PR)"
---

# Tighten This PR For Me

The request this skill encodes:

> Rewrite the PR description. Do not be overly descriptive, do not be overly informative, sound natural and neutral, do not generate slop, pass it all through /no-ai-slop.

## Inputs

- **PR URL or number:** read the live body with `gh pr view <n> --json title,body,files` and the diff with `gh pr diff <n>`. Never rebuild a body from memory.
- **Pasted draft:** use it as given.
- **Nothing:** use the PR for the current branch.

Also read the repo's PR template if one exists (`.github/PULL_REQUEST_TEMPLATE.md`, `.github/pull_request_template.md`, or `.github/PULL_REQUEST_TEMPLATE/`).

## Rules

1. **Template first.** Keep the template's headings, checkboxes, and HTML comments exactly as they are, because automation may read them. Fill each section.
2. **Say what changed and why.** Each prose section gets 1-3 plain sentences; count them before you return. Write about 60 words of your own prose and never more than 100. Template text, checklists, code, and reference lines do not count.
3. **Not overly descriptive.** Do not walk through the diff file by file. Do not explain mechanisms the diff already shows. The reviewer reads the diff.
4. **Not overly informative.** Cut background history, incident retelling, alternatives considered, tool or validator output, counts that will drift, and filler such as "No linked issue" or "N/A".
5. **Natural and neutral.** Use plain declarative sentences. No verdicts, no selling, no loaded words such as "robust", "crucial", "backwards", or "seamless". Do not open with "This PR introduces".
6. **Keep the substance.** Every issue reference, ID, version, flag name, and user-visible behavior change stays. Keep the repo's `Closes`/`Fixes`/`Refs` style.
7. **Testing steps are short and concrete.** For copy-only or docs-only changes, replace all test steps with the single line "Read the diff.", even when the original lists more.
8. **Plain formatting.** No bold lead-ins on bullets, no emoji, no headings beyond the template's.

## Workflow

1. Read the inputs and the template.
2. Draft the body against the rules.
3. Always run the draft through the `no-ai-slop` skill in edit mode, even when it already looks clean, and apply its edits.
4. Check the word limit again after the edit.
5. Return the final body in a fenced block with nothing before it. After the block, add a single line that starts with "Cut:" and lists what you removed in under 20 words (for example, "Cut: file walkthrough, background paragraph, test counts"). Add nothing else.
6. Do not edit the PR unless the user asks. When they ask, fetch the live body again, then run `gh pr edit <n> --body-file <file>`.

## Example

Before (typical agent draft):

```markdown
## Changes proposed in this Pull Request
This PR introduces a comprehensive refactor of the webhook retry logic to ensure robust handling of transient failures. Previously, the system would retry failed webhooks indefinitely, which could lead to significant queue buildup and degraded performance across the platform.

Key changes:
- **Retry cap:** Added a maximum of 5 retries in `class-webhook-retry.php`
- **Backoff:** Implemented exponential backoff with jitter
- **Logging:** Enhanced logging to capture failure reasons

This change is crucial for platform stability. Closes #123.
```

After:

```markdown
## Changes proposed in this Pull Request
Failed webhooks were retried forever and filled the queue. Retries now stop after 5 attempts with exponential backoff, and the last attempt logs the failure reason.

Closes #123.
```

## Common mistakes

- Rewriting from memory or an old copy instead of the live PR body.
- Dropping template checkboxes or HTML markers along with the fluff.
- Cutting facts (issue references, versions, flag names) instead of filler.
- Swapping one kind of slop for another ("This change streamlines...").
- Editing the PR on GitHub without being asked.
