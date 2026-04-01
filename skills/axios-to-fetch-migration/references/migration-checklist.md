# Axios -> Fetch Migration Checklist

## API mapping

- `axios.get(url, config)` -> `fetch(url, { method: 'GET', ...configToFetch })`
- `axios.post(url, data, config)` -> `fetch(url, { method: 'POST', body: serialize(data), ... })`
- `axios.put/patch/delete` -> corresponding HTTP method
- `axios.request(config)` -> wrapper `request(config)` backed by fetch

## Important semantic differences

1. Fetch does not reject on HTTP 4xx/5xx by default. Add explicit status checks.
2. Fetch has no built-in timeout. Implement with `AbortController` + timer.
3. Request/response transforms are not built-in. Implement in wrapper.
4. `response.data` convenience is absent; parse explicitly (`json/text/blob`).
5. Error shapes differ significantly; normalize to app contract.
6. In browser, cookie behavior depends on `credentials` option.
7. In Node.js, ensure runtime supports `fetch` (Node 18+ or polyfill).

## Interceptors migration

Replace axios interceptors with wrapper hooks:

- beforeRequest(config)
- afterResponse(response, parsed)
- onError(error, context)

Support ordered registration if project depends on interceptor sequencing.

## Cancellation migration

- Replace `CancelToken.source()` with `new AbortController()`
- Pass `signal` into fetch
- Distinguish timeout-abort vs user-abort in normalized errors

## Config conversion guide

- `baseURL` + `url` -> URL resolver
- `params` -> `URLSearchParams`
- `headers` -> `Headers` init
- `auth` -> Basic auth header helper (if needed)
- `withCredentials` -> `credentials: 'include'` (browser)
- `responseType` -> choose parser (`json/text/blob/arrayBuffer`)

## Testing matrix

For each migrated endpoint, test:

- 2xx with expected payload
- 4xx/5xx with server payload preserved
- network error
- timeout
- cancellation
- auth/header injection
- query parameter encoding

## Decommission

- remove `axios` from dependencies
- remove axios mocks and fixtures
- remove axios-specific docs/snippets
- run full CI and smoke test key user journeys
