---
name: tighten-pr-bloated
tags: [tighten, pr]
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
---

Rewrite this PR description. Do not be overly descriptive, do not be overly informative, sound natural and neutral, do not generate slop, pass all through /no-ai-slop.

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

This PR introduces a comprehensive overhaul of the webhook retry mechanism to ensure robust and reliable delivery of webhook events — a crucial improvement for platform stability.

**Background:** Historically, our webhook delivery system would retry failed deliveries indefinitely. Back in March, this led to an incident where a single misconfigured customer endpoint generated over 40,000 queued jobs, slowing down the job queue for everyone on the site. We considered several alternatives, including a circuit breaker and a dead-letter queue, but ultimately settled on a simpler capped approach.

**Key changes:**
- **`webhook.php`:** Added a retry counter stored with each webhook.
- **`webhook-retry.php`:** New class that implements exponential backoff (1, 2, 4, 8, 16 minutes) with jitter.
- **`webhook-logger.php`:** Enhanced logging so the final failure reason is captured.
- **Feature flag:** Everything is gated behind the `webhook_retry_cap` option so sites can opt out.

After 5 failed attempts, the webhook is marked as failed and an admin notice is shown. This ships in 9.4.0.

I ran PHPCS and PHPStan locally and both passed with zero errors. All 1,284 unit tests pass.

Closes #4521

### How to test the changes in this Pull Request

1. Create a webhook pointing to an endpoint that always returns HTTP 500.
2. Trigger an order update to fire the webhook.
3. Open Admin > Jobs and confirm there are exactly 5 attempts with increasing delays.
4. Confirm the webhook status is "Failed" and an admin notice appears.
5. Turn off `webhook_retry_cap` and confirm retries are no longer capped.

### Changelog entry

- [x] This change needs a changelog entry.
<!-- changelog-type: fix | add | update | dev -->
```
