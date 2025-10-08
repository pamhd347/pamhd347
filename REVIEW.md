# Code Review

## Security
- The `window.addEventListener('message', ...)` handler trusts any `postMessage` without checking `event.origin`. A malicious iframe could trigger polling for arbitrary job IDs or spam the API. Please validate that messages originate from your domain (e.g., by comparing `event.origin`) before acting on them.

## Bugs
- `copyToClipboard` wraps `navigator.clipboard.writeText` in a synchronous `try/catch`, but `writeText` is asynchronous. Rejections happen after the function returns, so the `catch` never runs and the success alert is shown even when copying fails. Use `await navigator.clipboard.writeText(textToCopy)` (inside an async function) or chain `.then/.catch` to show accurate feedback.

## Cleanup
- `summarizeResults` is defined but never called. Consider removing it or wiring it into the UI to avoid dead code.
