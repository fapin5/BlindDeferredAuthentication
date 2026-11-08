# AI Implementation Prompt (Cursor, Copilot, Claude, etc.)

**Context:** You are a Senior Software Security Engineer. Your task is to implement the "Blind Deferred Authentication" architecture in this project.

## Step 1: Read and Analyze
* Analyze the current authentication and login structure of this repository/project.
* Identify exactly where the credential validation occurs and where the final session/token (e.g., JWT) is issued.

## Step 2: Planning (Present to the user before coding)
Document a brief action plan detailing how you will introduce the following changes without breaking existing logic:
1. Intercept the login request.
2. Silently validate the password and save the state (success/fail) in a fast cache (e.g., Redis) with a strict 10-second time lock.
3. IMMEDIATELY return an `HTTP 202 Accepted` with a temporary token/cookie.
4. Create the invalidation rule/middleware:
   - If a request arrives with the temporary token BEFORE the 10 seconds are up, assume it is an impatient bot, instantly DELETE the cache, and return `401`.
   - If it arrives AFTER 10s and the status is successful, issue the real token.

## Step 3: Implementation
* Add the required Backend code (state management, non-blocking asynchronous locks, and the invalidation trigger).
* Add the required Frontend code (intercept the login form, display a 10s loading screen, and trigger the refresh/polling request after the wait).

## Step 4: Testing
* Create or document test scenarios demonstrating how a bot making requests under 10 seconds loses a valid session.

---
**Developed / Conceived by:** [bluetec.dev.br](https://bluetec.dev.br)
