---
name: x-api-mcp-guide
description: Always read this before the first call through the X MCP or plugin in a session
---

# X API / X MCP guide

The X API bills per request from a prepaid credit balance, so blind
retries cost real money and the errors below are the normal way the API tells
you which account precondition is missing.

## The four preconditions

Every successful call requires ALL of these at once. Each missing one maps to
a distinct error:

1. **Developer account** exists (signed up at console.x.com, agreement accepted).
2. **App is attached to a Project** — a standalone app fails even with valid keys.
3. **Valid credentials** of the right kind for the endpoint (see Auth modes).
4. **Positive credit balance** — pay-per-use, no free tier. Balance can go
   slightly negative; requests stay blocked until the shortfall is covered.

## Error decode table

Match top-down; the first row that fits is usually the real cause.

| HTTP | Error type / reason | Account state | Fix |
| --- | --- | --- | --- |
| 401 | `about:blank`, bare `"detail": "Unauthorized"` | No developer account, wrong/regenerated/revoked keys, or a suspended app. Deliberately vague. | Verify credential freshness and type; check app status in the Developer Console. |
| 403 | `client-forbidden` / `"reason": "client-not-enrolled"` | Enrolled, but the app is not attached to a Project (or was silently detached — a known recurring bug). | Developer Console → app settings → attach to a Project ("Move to package" if no Project UI). |
| 403 | `not-authorized-for-resource` or plain Forbidden | Wrong auth mode: app-only Bearer Token on a write endpoint, missing OAuth scope, or private/protected resource. | Use a user-context token with the needed scopes. |
| 402 | Payment Required (NOT in the official status table) | Enrolled but credit balance is zero or negative. Message: "Your enrolled account [id] does not have any credits to fulfill this request." | Buy credits in the console. Do not retry until topped up. |
| 429 | `rate-limit-exceeded` | Per-endpoint rate limit hit (15-min or 24-h window, per app or per user token). | Wait until the `x-rate-limit-reset` Unix timestamp; back off exponentially. |
| 429 | `usage-capped` | Monthly cap hit (3M post reads) or spending limit for the billing cycle reached. | Wait for cycle reset, raise the spending limit, or Enterprise. |
| 409 | `streaming-connection` conflict | Filtered stream has no rules defined. | Add at least one stream rule before connecting. |
| 400 | `invalid-request` | Malformed request, not an account problem. | Fix params/JSON. Never retry unchanged. |
| 5xx | — | X-side outage, not your account. | Retry with backoff; check developer.x.com/status. |

A 200 can still contain a partial `errors` array next to `data` (multi-resource
lookups where some resources are gone/protected). Always check for it.

## Auth modes

- **Bearer Token (app-only)**: public reads only. Writing with it returns 403
  even on a fully funded account.
- **OAuth 2.0 user context / OAuth 1.0a**: required for posting, likes,
  follows, DMs. OAuth 2.0 user tokens expire in ~2 hours — implement refresh.
- Credentials are shown once at creation; regenerating invalidates old keys
  (that shows up as the bare 401 above).

## Cost-aware behavior for agents

- Errors are free (only successful data-returning responses are billed), but
  successes cost real credits: ~$0.005/post read, $0.015/post create,
  $0.200/post containing a URL. Do not loop reads speculatively.
- Identical resources are deduplicated within a 24-hour UTC day (soft
  guarantee) — re-reading the same post the same day is free-ish; the next
  day it bills again.
- On 402, stop and tell the user to add credits; auto-recharge fires at most
  once per 5 minutes and pauses entirely at zero/negative balance, so bursts
  can 402 even with auto-recharge on.
- Retry policy: backoff-retry only 429 and 5xx. Never retry 400/401/402/403
  without changing something.

## Reference payloads

Not enrolled / app not in a Project (403):

```json
{
  "title": "Client Forbidden",
  "reason": "client-not-enrolled",
  "required_enrollment": "Appropriate Level of API Access",
  "detail": "When authenticating requests to the Twitter API v2 endpoints, you must use keys and tokens from a Twitter developer App that is attached to a Project. ...",
  "registration_url": "https://developer.twitter.com/en/portal/opt-in",
  "type": "https://api.twitter.com/2/problems/client-forbidden"
}
```

Out of credits (402, wording from live API; absent from official docs):

```text
Your enrolled account [1146115155501039616] does not have any credits to fulfill this request.
```

Every response carries `x-rate-limit-limit`, `x-rate-limit-remaining`, and
`x-rate-limit-reset` headers; rate limits are separate from billing.

Full docs: append `.md` to any docs.x.com URL for clean markdown, e.g.
`https://docs.x.com/x-api/fundamentals/response-codes-and-errors.md` and
`https://docs.x.com/x-api/getting-started/pricing.md`.
