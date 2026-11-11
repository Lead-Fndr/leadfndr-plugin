---
name: leadfndr-getting-started
description: Connect to Lead Fndr with an Assistant key, check whoami and scopes, and follow spend/send/generate safety rules.
---

# Connect to Lead Fndr

Use this when the user wants to hook up Lead Fndr, check their key, or make a first read-only call.

## Connect

1. In Lead Fndr, open **Integrations → API & Assistant** (`/settings/integration/api`).
2. Create an **Assistant key**. Start with the **Read** scope.
3. The plugin sends it as `Authorization: Bearer <key>` to `https://api.leadfndr.com/api/v1/mcp`. Never embed, log, or repeat the key.
4. A browser sign-in token is refused. An Assistant key (has scopes) is the right key; a plain API key is read-only on this connection.

## First calls

- `whoami` — the signed-in account and the scopes this key may use.
- `get_wallet` / `list_wallet_transactions` / `get_prices` — balance, ledger, and effective prices (prices are hidden when the account does not show pricing).
- `get_dashboard` — overview for `week` | `month` | `year` | `all`.
- `list_lists` / `list_recent_searches` — a safe way to confirm access.

## Safety (every spend, send, or generate call)

1. Check scope first. A read key calling a spend tool is refused and charges nothing.
2. Without `confirm: true`, the call is a dry run (estimate only, no write, no charge, no send, no generate).
3. With `confirm: true`, also pass the cap or the call must not proceed:
   - spend: `max_cost` (USD this call may charge)
   - send: `max_leads`
   - generate: `max_generations`
4. If the recomputed estimate is above the cap, nothing is charged or sent.
5. Deletes and disconnects need `confirm: true` or they write nothing.
6. List and get tools for integrations and CRM connections never return secrets. Do not ask the user to paste secrets into a tool.

`create_agent` and `update_agent` cannot set an agent live. Use `set_agent_status` after the search and canvas are ready.

When saving leads, a list's active "new lead" automations may start after the call. Their estimate is included in `max_cost`; a cap that covers the save but not them is refused.

## Limits

60 calls per minute per account; 10 confirmed spend/send/generate calls per minute. `429` includes `Retry-After`.
