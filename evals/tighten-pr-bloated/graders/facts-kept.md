---
type: llm
weight: 1
---

Judge only the rewritten PR body, not any note about what was cut. The body still says that retries stop after 5 attempts with exponential backoff, that the webhook is then marked failed with an admin notice, that the `webhook_retry_cap` option turns the cap off, that it ships in 9.4.0, and that it closes #4521. The test steps still cover a failing endpoint, checking for 5 attempts, the failed status or admin notice, and turning the option off.
