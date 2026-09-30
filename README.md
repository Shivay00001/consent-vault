# ConsentVault — interactive U-PERM demo

A single-file, dependency-free web prototype demonstrating **U-PERM**, the universal
permission layer from the U-STACK architecture:

> **User-owned data. Apps as guests. Capabilities, not ambient authority.**

Scenario: you own a clinic. Your AI receptionist agent (`agent 'recep-01'`, your principal)
needs data access. You grant it **scoped, time-bound capability tokens** — then hit
**REVOKE ALL** and watch its access die live.

## What it demonstrates (U-PERM invariants)

| Invariant | How the demo shows it |
|---|---|
| **P1 — default-deny** | With zero grants, every agent attempt is red **DENIED** ("no grants issued — default-deny"). The onboarding hint reads *"Issue a grant to let the agent work."* |
| **P2 — attenuation** | Grants are minted per-resource with explicit read/write toggles. A grant for *read appointments* does **not** authorize *write billing* — out-of-scope attempts are denied with "attenuation holds". |
| **P3 — time-bounded** | Every grant carries an expiry. Issue a **60-second** grant and watch it count down live on its token card, then auto-expire → subsequent attempts DENIED and the audit log records *"grant expired"*. |
| **P4 — total revocation** | The big red **REVOKE ALL** button instantly revokes every live grant: red screen flash, banner *"All access revoked — agent is now blind"*, token cards flip to **REVOKED**, and the feed flips to all-DENIED. Per-grant Revoke buttons do the same for one token. |
| **P5 — legible purpose** | Every grant carries a plain-language purpose (default *"Handle appointment bookings"*), printed on the token card and in the audit log. |

## Layout

- **Left — Grant builder:** resource checkboxes (Appointments calendar, Patient records, Billing ledger) with per-resource read/write pills, expiry select, purpose field, and "Issue grant". Each issued grant becomes a capability token card: `issuer → holder`, resource/action chips, live expiry countdown, per-grant Revoke.
- **Middle — Agent activity:** simulated receptionist attempts an action every ~2.5s (read/write × 3 resources). Each is checked against live grants → green **ALLOWED** (citing the authorizing grant) or red **DENIED** (citing the reason). Start/Pause button.
- **Right — Audit log:** append-only record of every grant issued, access allowed/denied, grant expired, grant revoked — with timestamps and a "Copy audit log" button.

## How to run

No build, no dependencies, no network. Either:

1. **Double-click** `index.html` (works over `file://`), or
2. ```bash
   cd ~/workspace/inventions/consent-vault && python3 -m http.server 8000
   ```
   then open http://localhost:8000 in a browser.

## Demo script (5 steps)

1. **Watch the blind agent.** On load, zero grants exist — every feed row is red DENIED (P1).
2. **Grant narrow access.** Check *Appointments calendar* → read only, expiry 10 minutes, purpose *"Handle appointment bookings"*, Issue grant. Feed rows for appointment reads flip green; billing writes stay red (P2).
3. **Watch a grant die.** Issue a *60-second* grant for *Billing ledger* read. Its card counts down live; when it hits zero, billing attempts flip back to DENIED and the audit log records the expiry (P3).
4. **Check the audit log.** Every event is timestamped and append-only — the receipt trail.
5. **REVOKE ALL.** Hit the big red button. Red flash, banner *"All access revoked — agent is now blind"*, cards flip to REVOKED, feed goes all-DENIED (P4). The money moment.

## Notes

- Everything is **simulated client-side**: the agent, the authorization checks, the grants,
  and the audit log. No real data, no network calls, no storage beyond the page session.
- Honest footer included in the page: *"Interactive simulation of the U-STACK U-PERM
  specification · No real data · VisionQuantech research demo"*.
