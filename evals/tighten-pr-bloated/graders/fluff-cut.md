---
type: llm
weight: 1
---

Judge only the rewritten PR body, not any note about what was cut. The body drops the incident story (March, 40,000 jobs), the alternatives considered (circuit breaker, dead-letter queue), the file-by-file walkthrough, and the PHPCS, PHPStan, and unit-test report. Not counting the template headings, HTML comments, checklist, test steps, and the Closes line, the body has at most 100 words of prose, in 1-3 plain sentences. The tone is neutral: it does not open with "This PR introduces" and uses no puffery such as "comprehensive", "robust", "crucial", or "seamless".
