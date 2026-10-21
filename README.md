# LimitYourAPI examples

Integration examples for [LimitYourAPI](https://limityourapi.tech), a hosted API rate limiting service I built. Your server asks the service whether to serve a request. The service runs the matching rule as one atomic Redis Lua script and answers allow or deny.

The service and the SDK source live in a private repository. This one holds the examples.

[Website](https://limityourapi.tech) · [Docs](https://limityourapi.tech/docs) · [npm package](https://www.npmjs.com/package/limityourapi)

## Examples

| Folder | Framework | Client |
| :-- | :-- | :-- |
| [`examples/node-express`](examples/node-express/server.js) | Express | `limityourapi` from npm |
| [`examples/nextjs`](examples/nextjs/middleware.ts) | Next.js middleware | `limityourapi` from npm |
| [`examples/fastapi`](examples/fastapi/main.py) | FastAPI | Python SDK, not on PyPI yet |
| [`examples/go`](examples/go/main.go) | Go (Fiber) | Go SDK, not published yet |

Each example checks the request path before the route runs, sets the `X-RateLimit-*` headers and returns 429 with `Retry-After` when the limit is hit.

## Node.js

```bash
npm install limityourapi
```

Check a route against the rules you set in the dashboard:

```js
import { LimitYourAPIClient } from 'limityourapi';

const limiter = new LimitYourAPIClient({ apiKey: process.env.LIMIT_YOUR_API_KEY });

const result = await limiter.check({ endpoint: '/v1/items' });
if (!result.allowed) {
  // reply 429 and set Retry-After to result.retryAfter
}
```

Count usage against a customer's plan (version 3 and later):

```js
const result = await limiter.enforce({ customer: 'user_123', meter: 'api_calls', consume: 1 });
```

If the service doesn't answer before the timeout (5 seconds by default), the SDK lets the request through. Pass `failOpen: false` to block instead.

## Python and Go

Both clients are written but not published yet, so `pip install` and `go get` won't work today. Until they are, call the REST API directly:

```bash
curl -X POST https://api.v2.limityourapi.tech/check \
  -H "X-API-Key: $LIMIT_YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"endpoint": "/v1/items"}'
```

## How the service works

- A Go (Gin) API. Each decision is one Redis Lua script, so two requests at the same moment can't both take the last token.
- Token bucket, sliding window and quota rules, plus limits by token count and cost for AI APIs.
- Request logs are buffered in memory and written to PostgreSQL in batches, off the request path.
- Customer quotas can follow plans in Stripe or Razorpay through webhooks.

No latency benchmarks of the hosted service have been published. I measured the Lua approach itself in [Atomic rate limiting with Redis and Lua, measured](https://www.yashsuthar.dev/writing/atomic-rate-limiting-redis-lua).

## License

The examples are under the [Apache 2.0 License](LICENSE). The `limityourapi` npm package is MIT.
