---
name: axios-to-fetch-migration
description: Migrate JavaScript and TypeScript codebases from axios to fetch with behavior-preserving refactors, compatibility wrappers, and test-first validation. Use when replacing axios.get/post/request/create/interceptors/cancel-token usage, handling response and error-shape differences, or removing axios dependency safely.
---

# Axios to Fetch Migration

Follow this workflow to migrate axios to fetch without behavior regressions.

## 1) Inventory axios usage

Run a fast code search first.

```bash
rg "from ['\"]axios['\"]|axios\.|AxiosError|CancelToken|interceptors|axios\.create|axios\.request" -n .
```

Classify each hit:

- Simple calls: `axios.get/post/put/delete`
- Config-heavy calls: timeout, params, headers, auth, baseURL
- Shared clients: `axios.create(...)`
- Interceptors: request/response hooks
- Cancellation: `CancelToken` or `signal`
- Error handling tied to axios shape: `error.response`, `error.request`, `error.code`

If repo has many files, prioritize hot paths and tested modules first.

## 2) Choose migration strategy

Prefer one of these:

- Big-bang: small repo, strong test coverage
- Strangler: add a fetch-based HTTP adapter, migrate module-by-module

Default to strangler for medium/large repos.

## 2.5) Ensure `fetch` is available in your runtime (Node.js)

Before migrating lots of call sites, confirm your runtime actually provides `fetch`.

- Node 18+: `fetch` is available globally.
- Node 16: add a polyfill (recommended: `undici@5`) or build your wrapper on top of `undici`.

Details and copy/paste snippets: `references/migration-checklist.md`.

## 3) Establish a fetch wrapper before mass edits

Create one shared HTTP utility that normalizes behavior.

Required wrapper behavior:

- JSON default parsing and optional text/blob parsing
- Throw on non-2xx with consistent error object
- Query param encoding
- Timeout support via `AbortController`
- Default headers and base URL joining
- Optional middleware hooks to replace interceptors

Keep call sites thin; move complexity into wrapper.

## 4) Map axios semantics to fetch explicitly

Use `references/migration-checklist.md` as the source of truth.

Common mappings:

- `axios({ url, method, data, params })` -> `fetch(urlWithParams, { method, body, headers, signal })`
- `response.data` -> parsed body from wrapper
- `validateStatus` -> wrapper status policy
- `timeout` -> `AbortController` timer
- `axios.CancelToken` -> `AbortController`
- `axios.interceptors` -> wrapper middleware/hook pipeline

## 5) Refactor safely in slices

For each slice:

1. Add/adjust tests first (unit/integration)
2. Migrate imports and callsites
3. Remove axios-specific assumptions
4. Run tests and lint
5. Commit small logical changes

Avoid broad regex rewrites without tests.

## 6) Error and response normalization

Ensure app code does not rely on axios-only fields.

Adopt one app-level error contract, e.g.:

- `name`, `message`, `status`, `code`, `url`, `method`, `cause`
- parsed server payload on HTTP errors when available

Update callsites to consume this normalized shape.

## 7) Remove dependency and dead code

After all callsites migrate:

- remove axios package
- remove axios plugins/interceptor utilities
- delete obsolete types and mocks
- run full test suite and typecheck

## 8) Verification checklist

Before declaring done:

- no runtime import of axios remains
- retries/timeouts/cancellation still work
- auth headers still injected
- non-2xx behavior matches intended product logic
- tests cover success, HTTP error, network error, timeout, cancellation

Use this command for final sweep:

```bash
rg "from ['\"]axios['\"]|require\(['\"]axios['\"]\)|axios\." -n .
```

Expect zero relevant hits.

## 9) Add targeted tests for changed code

For each modified area, add 1-2 focused tests to lock behavior:

- At least one happy-path test for the migrated request flow
- One edge-case test (choose one): HTTP error, network failure, timeout, or cancellation

Keep tests close to the changed module and verify they fail before the fix and pass after it.

## References

- Migration details and edge cases: `references/migration-checklist.md`
