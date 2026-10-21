# Changelog

Versions of the `limityourapi` npm package.

## 3.2.0 (2026-08-25)

- Added `meterAI()` to count LLM token usage in one call.
- Added `injectHeaders()` to set `X-RateLimit-*` and `Retry-After` on Express, Fastify or Web responses.
- No runtime dependencies. The SDK uses the built-in `fetch`.

## 3.0.1 (2026-06-23)

- Added `enforce()` to count usage by customer and meter: requests, AI tokens or cost.
- Quotas can follow a customer's plan in Stripe or Razorpay, through webhooks to the service.
- `check()` still works. `check("id")` is the same as `enforce({ customer: "id", meter: "api_calls", consume: 1 })`.

## 2.0.1 (2026-06-18)

- The package is now published as `limityourapi`.
- Added parameter aliases (`route`, `key`, `identifier`) and response aliases (`reset`, `retryAfter`, `retry_after`).
- Added TypeScript types: `LimitYourAPIOptions`, `CheckOptions`, `CheckResult`.
- Added the Express, FastAPI and Go examples in this repository.

## 1.0.0 (2026-04-13)

- First release of the Node.js SDK, with Express middleware.
