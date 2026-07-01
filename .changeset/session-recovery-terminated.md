---
"@moonshot-ai/kosong": patch
"@moonshot-ai/agent-core": patch
"@moonshot-ai/kimi-code": patch
---

Recover the session after a mid-stream provider connection drop (`terminated`) instead of leaving it unusable:

- Classify a raw dropped streaming connection (undici `terminated`) as a retryable `APIConnectionError` on the Anthropic path — previously it fell through to a fatal generic error and was never retried.
- Return the TUI to an input-ready state when a request-level error arrives while no turn is actually running, so the next message is sent instead of being silently swallowed by the queue. Errors raised while a turn is streaming are still left to `turn.ended`.
- Release a session's in-memory active-prompt slot when the session closes, so a reopened/resumed session can no longer be wedged by a stale in-flight prompt.
