---
name: leadfndr-sub-accounts
description: Let a Lead Fndr agency owner list paying client sub-accounts and run tools in a client account with as_sub_account_id.
---

# Act in client sub-accounts

Use this when an agency owner wants to work in a paying client's Lead Fndr account.

Client accounts are **sub-accounts**. Never call them anything else.

## Roster (agency book only)

These ignore `as_sub_account_id` and stay on the agency roster:

- `list_sub_accounts`
- `get_sub_account`
- `get_sub_account_analytics`

A client or staff key passing `as_sub_account_id` is forbidden. A foreign sub-account is not found. An agency owner whose plan does not include Agency features is plan-restricted.

## Act as a client (per call)

An Assistant key works in the account of the session that created it. An agency owner can pass `as_sub_account_id` on a tool for lists, leads, search, spend, send, agents, Skills, profile, generate, CRM, `whoami`, wallet, prices, or dashboard.

That call runs in the paying client's account: the **client wallet** and the **client daily generate quota** are used. The audit row still names the agency owner as the actor and the client as the payer. This does **not** mint a sign-in token.

`login_as_sub_account` (`write`) records one impersonation-log row and returns `{ subAccountId, name, ownerEmail }` — no token. Other tools still need `as_sub_account_id` on each call (the connection is stateless).
