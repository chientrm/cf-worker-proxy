# cf-workers-proxy

[![Download](https://img.shields.io/npm/dt/cf-workers-proxy)](https://www.npmjs.com/package/cf-workers-proxy)
[![Version](https://img.shields.io/npm/v/cf-workers-proxy)](https://github.com/chientrm/cf-workers-proxy)

Enable Cloudflare Workers runtime for local development. Compatible with DrizzleORM.

## Get Started

### Install wrangler

```
npm i -D wrangler
```

### Install `cf-workers-proxy`

```
npm i -D cf-workers-proxy
```

### Example `wrangler.toml`

```toml
name = "worker"
compatibility_date = "2023-07-02"

[[d1_databases]]
binding = "D1"
database_name = "D1"
database_id = "<d1-id>"

[[kv_namespaces]]
binding = "KV"
id = "<kv-id>"
preview_id = "<same-kv-id-as-above>"

[[services]]
binding = "WORKER"
service = "<worker-name>"
environment = "production"
```

### Start proxy

```
npx wrangler node_modules/cf-workers-proxy/dist/worker.js --remote
```

### Example SvelteKit project

```ts
// file: app.d.ts

declare global {
  namespace App {
    interface Locals {
      D1: D1Database;
    }
    interface Platform {
      env?: {
        D1: D1Database;
      };
    }
  }
}

export {};
```

```ts
// file: src/hooks.server.ts

import { createD1 } from 'cf-workers-proxy';

export const handle = ({ event, resolve }) => {
  event.locals.D1 = event.platform?.env?.D1 ?? createD1('D1');
  // or createD1('D1', { hostname: 'custom-host-name' });
  // default hostname is `http://127.0.0.1:8787`
  return resolve(event);
};
```

## Roadmap

- ❌ Not started
- 🟡 Partially implemented
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
| `raw()`   | ✅     |
| `bind()`  | ✅     |

### Service Bindings

```ts
import { createServiceBinding } from 'cf-workers-proxy';
```

| Function    | Status |
| ----------- | ------ |
| `fetch()`   | ✅     |
| `connect()` | ❌     |

### KVNamespace

```ts
import { createKV } from 'cf-workers-proxy';
```

| Function            | Status |
| ------------------- | ------ |
| `put()`             | 🟡     |
| `get()`             | 🟡     |
| `getWithMetadata()` | ❌     |
| `delete()`          | ❌     |
| `list()`            | ❌     |

### `waitUntil`

```ts
// file: app.d.ts
namespace App {
  interface Platform {
    context: {
      waitUntil(promise: Promise<any>): void;
    };
  }
}
```

```ts
// file: +page.server.ts
import { waitUntil } from 'cf-workers-proxy';

export const actions = {
  default: ({ locals, platform }) => {
    waitUntil(async () => {}, platform?.context.waitUntil);
    return { message: 'success' };
  },
};
```

## Contributing

<a href="https://www.buymeacoffee.com/chientrm" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>

or pull request 😐
