# cf-workers-proxy

Enable Cloudflare Workers runtime for local development.

## How to use?

### Setup proxy server

- [Setup Wrangler server](docs/server.md)

### Setup client

- [Setup for SvelteKit project](docs/sveltekit.md)

## Roadmap

- ❌ Not started
- ✅ Complete

### D1Database

```ts
import { createD1 } from 'cf-workers-proxy';
```

| Function    | Status |
| ----------- | ------ |
| `prepare()` | ✅     |
| `batch()`   | ❌     |
| `dump()`    | ❌     |
| `exec()`    | ✅     |

### D1PreparedStatement

| Function  | Status |
| --------- | ------ |
| `first()` | ✅     |
| `run()`   | ✅     |
| `all()`   | ✅     |
| `raw()`   | ❌     |
| `bind()`  | ✅     |

### Service Bindings

```ts
import { createServiceBinding } from 'cf-workers-proxy';
```

| Function    | Status |
| ----------- | ------ |
| `fetch()`   | ✅     |
| `connect()` | ❌     |

## Contributing

Just pull request 😐
