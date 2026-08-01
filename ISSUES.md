# Issue Publication Status

This file is the sole source of truth for every finding's permanent ID, lifecycle status, and published
location. Read and update this file instead of inferring state from chat history or earlier research.

Next finding ID: ISSUE-2026-009

## Internal Download Issues

### ISSUE-2026-001 — ota: Pallas requests create a separate HTTP transport per request

- The unfiltered iOS path expands three non-empty audiences across 385 current board IDs, yielding 1,155 jobs.
- `sendPostAsync` creates a client, transport, certificate pool, and TLS configuration for every job.
- Reuse is source-proven; Pallas negotiated HTTP/1.1, while latency and allocation impact remain unmeasured.

Status: Drafted for a new enhancement issue — prior-art research completed; exact draft awaits user approval.
Location: Not published.

### ISSUE-2026-002 — ota: Pallas response error paths leave bodies open

- `GetPallasOTAs` closes a response body only at the end of the successful decode-and-assets path.
- Status, read, base64, JSON, and empty-assets branches can continue before `resp.Body.Close()`.
- The lifetime gap is source-proven; open-body growth and failure-path frequency are not measured.

Status: Hold — source-proven; resource growth not measured and upstream duplicate research not completed.
Location: Not published.

### ISSUE-2026-003 — appledb: metadata helpers create a separate HTTP transport per call

- `AppleDBQuery` invokes `queryGithubAPI` and `getOsFiles` repeatedly while traversing matching metadata.
- Both helpers construct their own client and transport instead of sharing query-scoped request infrastructure.
- Repeated construction is source-proven; request count, handshake count, and latency are not measured.

Status: Hold — source-proven; performance impact not measured and upstream duplicate research not completed.
Location: Not published.

### ISSUE-2026-004 — downloader: batch items cannot share pooled HTTP connections

- AppleDB, OTA, and wiki batch loops create a separate `Download` object for every selected item.
- `NewDownload` couples each item to a new client and transport although item state is otherwise independent.
- Per-item transport ownership is source-proven; batch sizes, connection reuse, and latency are not measured.

Status: Hold — source-proven; performance impact not measured and upstream duplicate research not completed.
Location: Not published.

### ISSUE-2026-005 — ota: uniqueOTAs rescans every asset for every unique key

- `uniqueOTAs` first records unique `BaseURL+RelativePath` keys and then scans all input assets per result.
- Merge helpers add further linear membership scans and device sorting inside the duplicate-group traversal.
- The `O(U×N)` control flow is source-proven; real asset cardinality, CPU time, and allocations are not measured.

Status: Hold — source-proven; performance impact not measured and upstream duplicate research not completed.
Location: Not published.

## App Store Issues

### ISSUE-2026-006 — appstore: multi-step workflows rebuild HTTP transports per method

- Provisioning, profile creation, renewal, and capability workflows call several methods on one `AppStore`.
- The network methods create separate clients and transports while authorization remains request-header state.
- Lost cross-method pooling is source-proven; workflow handshakes, latency, and allocation cost are not measured.

Status: Hold — source-proven; performance impact not measured and upstream duplicate research not completed.
Location: Not published.

## Web Issues

### ISSUE-2026-007 — entitlements: aborted searches continue work and can commit stale state

- `performSearch` aborts the previous controller but does not pass its signal to either Supabase search method.
- Every completion can still update results, errors, and loading state after a newer search or view reset.
- Missing propagation and stale-commit guards are source-proven; wasted query count and UI latency are not measured.

Status: Hold — source-proven; user-visible impact not reproduced and upstream duplicate research not completed.
Location: Not published.

## CLI Issues

### ISSUE-2026-008 — download wiki: JSON mode registers one database write per IPSW

- The filtered IPSW loop registers a deferred full-map marshal and write for every processed item.
- One exit-owned write can preserve partial-return persistence without repeating the same final serialization.
- Repeated write registration is source-proven; database sizes, write counts, and runtime impact are not measured.

Status: Hold — source-proven; performance impact not measured and upstream duplicate research not completed.
Location: Not published.
