# 🛡️ Blind Deferred Authentication

**An active defense architecture against brute-force attacks, focused on exhausting the resources (RAM/CPU) and patience of automated bots.**

## 💡 The Concept
It inverts traditional login logic by introducing a **mandatory and blind time toll** in the backend, coupled with an **impatience penalty**.

## ⚙️ How It Works (The Trap)
1. **The Request:** The client sends credentials.
2. **The Blind Response:** The server immediately returns an HTTP 202 Accepted status with a temporary token.
3. **The Freeze (Backend):** The server saves the validation result in a fast cache (e.g., Redis) with a **10-second time lock**. 
4. **The Impatience Penalty:** If the server receives any subsequent request using that token before the 10 seconds are up, the token is **incinerated**. 
5. **Legitimate Access:** The human user waits through the loading screen (10s), and the frontend refreshes automatically.

---
**Developed / Conceived by:** [bluetec.dev.br](https://bluetec.dev.br)
