# Élan Boutique — Engineering deep-dive

> Companion to the [main README](./README.md). This is the "show your work" document: every notable decision, the tradeoff it buys, and how it's defended.

---

## Context in one paragraph

A real small business was running on social-media DMs. The brief was a real storefront with real inventory, no dedicated developer, no heavy platform, and a datastore the operator could still read. The result is a production system: Apps Script as the application layer, Sheets as the operational database, a Tailwind SPA, and an admin console with token auth. It is live, used daily, and maintained.

---

## 1. Concurrency: lock-serialized checkout

**Problem.** Two customers check out the last unit of the same variant. Naïve `getValues` → mutate → `setValue` double-decrements and oversells.

**Decision.** Wrap order creation in `LockService.getScriptLock().waitLock(15000)`. Inside the lock: re-read inventory, validate cumulative quantity per variant, apply changes to an in-memory map, write once, recompute derived `products.stock`.

**Tradeoff.** Throughput is bounded by the lock (~2 s per order under contention). Acceptable for a boutique; would not scale to flash sales. If volume grows, the migration path is a real transactional store (Postgres/Supabase) — named in the roadmap.

**Evidence.** `createOrder()` accumulates `pendingInvChanges` keyed by inventory row, so multiple line items for the same variant are checked *cumulatively*, not per line. Missing variants throw instead of silently overselling.

---

## 2. Caching: three layers, with a size guard

**Problem.** Apps Script read quotas and latency; Sheets reads are the dominant cost.

**Decision.** Three layers:
1. **`CacheService`** for the boot payload (settings + categories + products + inventory + FAQs) and public settings, 5 min TTL.
2. **Per-execution read cache** (`_sheetReadCache`) so a single request never reads the same sheet twice.
3. **`localStorage` mirror** on the client (5 min TTL, version-keyed) so the storefront renders from cache while the network catches up.

**Guard.** `CacheService` has a ~100 KB per-key limit; the boot payload is serialized and only written if `< 95 KB`, otherwise the request falls through to a direct read. Cache keys are versioned (`boot_data_v4`) and the client cache version is bumped on deploy, so a schema change cannot serve stale shapes.

**Tradeoff.** Up to 5 min staleness on storefront reads. Mitigated by explicit invalidation on every admin mutation (`invalidateBootCache()`).

---

## 3. Auth: tokens, throttling, graceful expiry

**Problem.** Admin CRUD endpoints were behind a `/exec` URL; a shared password alone is not a session model.

**Decision.**
- **Server-issued rotating tokens** stored in `Script Properties` with a 12 h TTL. Expired tokens are cleaned up on sign-in and deleted lazily on check.
- **Brute-force throttling:** 5 failed attempts → 15 min global lockout. *(Tradeoff: a global lockout is a DoS vector against the owner. For a one-operator boutique this is acceptable; a per-IP model would need client-IP plumbing Apps Script doesn't expose cleanly.)*
- **No hardcoded password.** The admin password lives only in `Script Properties` (`ADMIN_PASSWORD`); `getAdminPassword()` throws a clear error if it's missing. There is no fallback constant in source.
- **Graceful expiry on the client.** Every admin call goes through `adminRun()`, which detects `"No autorizado"`, clears `sessionStorage`, toasts "Sesión expirada", and routes to login — instead of leaving the operator on a broken dashboard.

**Tradeoff.** Tokens are bearer tokens with no per-device binding. Acceptable for a single-operator admin; would need device/IP binding for a multi-tenant panel.

---

## 4. XSS: escape at the boundary

**Problem.** Customer-supplied strings (name, address, notes, product names from the sheet) reach admin views *and* HTML emails.

**Decision.** A single `escapeHtml()` helper escapes `& < > " '` at every interpolation point in server-rendered HTML *and* in `sendOrderNotificationEmail()`. The storefront uses the same helper client-side.

**Why it matters.** The email path is the non-obvious one: an unescaped customer name in an HTML email is a phishing vector *into the owner's inbox*. This was caught in review and fixed by escaping every field before `MailApp.sendEmail`.

---

## 5. Data integrity: derived stock, immutable orders

**Problem.** A single `stock` column on `products` drifts from the variant-level truth as soon as variants exist.

**Decision.**
- **`inventory` is the source of truth** (size × color × stock).
- **`products.stock` is derived** and recalculated on every variant mutation (`recalcProductStock`) and on explicit "sync all stocks".
- **Order line items are snapshotted** (`product_name`, `variant_label`, `unit_price`) at purchase time. Renaming or repricing a product later does not rewrite history.
- **Order creation throws** if a cart item references a deleted product or an unavailable variant — no silent partial orders.

---

## 6. Failure modes I named and handled

| Failure | Handling |
|---|---|
| Cache over 100 KB | Size guard; fall through to direct read |
| Concurrent checkout | `LockService` + cumulative variant check |
| Deleted product mid-checkout | Explicit throw; customer sees a clear error |
| Expired admin token | `adminRun()` → clear session → login |
| Failed email send | `try/catch`; order still persists (email is best-effort) |
| Missing `ADMIN_PASSWORD` | Clear thrown error at auth time |
| Sheet renamed / missing | `readSheetFrom` returns `[]`; parsers tolerate absence |
| Stale client cache after deploy | Version-keyed `localStorage` + deploy-time bump |

---

## 7. What I'd do next (honest backlog)

- **Move off Sheets** if order volume outgrows the cache layer — the transaction model would become a Postgres row-level lock, not an Apps Script mutex.
- **Per-IP throttling** on admin login, replacing the global lockout.
- **Structured logging** (order ID + outcome) to a log sheet, for post-mortems.
- **Shipping/tax rules** — currently manual by design; the schema is ready for a `shipping_rules` sheet.
- **Tests.** The system is exercised in production; the next investment is a thin test harness around `createOrder` and the parsers (the two places a regression would hurt most).

---

## 8. Reading the code

- `code.gs` — API surface, parsers, `createOrder`, admin endpoints.
- `app.html` — global state, router, cart, boot.
- `components.html` — shared UI (header, footer, product card, icons).
- `pages.html` — storefront routes (home, catalog, product, cart, checkout, confirmation, FAQ, contact, assistant).
- `admin.html` — admin routes (login, dashboard, products, editor, orders, categories, settings) and the auth-error wrapper.

*(Source is private; these are the files described in the showcase.)*

---

<div align="center">
<sub>Built for a real business. Happy to walk through any decision above.</sub>
</div>