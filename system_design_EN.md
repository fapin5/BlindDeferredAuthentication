# System Design Document: Blind Deferred Authentication
Details the data flow for pattern implementation.

## 1. Execution Flow (State Machine)
### Phase 1: Initial Request and Cache
* Backend generates `Session_UUID` and saves in **State Cache**:
  * `key`: `pending_auth:{Session_UUID}`
  * `value`: `{ status: "success|fail", valid_after: Timestamp + 10s }`

### Phase 2: The Blind Response
* `Set-Cookie: pending_session={Session_UUID}; HttpOnly`
* `Status Code: 202 Accepted` (Immediate)

### Phase 3: The Invalidation Trigger (The Trap)
1. Compare current time with `valid_after`:
   * **SCENARIO A (Bot - Before 10s):** IMMEDIATELY delete key from cache. Return `401`.
   * **SCENARIO B (Human - After 10s):** Check `status`. If `success`, issue final token.

---
**Documentation maintained by bluetec.dev.br**
