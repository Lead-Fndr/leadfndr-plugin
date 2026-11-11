---
name: leadfndr-send-leads
description: Send Lead Fndr leads to an existing webhook or CRM connection with confirm and max_leads. Secrets are never returned.
---

# Send leads to CRM or webhooks

Use this when the user wants to connect a destination or send leads out of Lead Fndr.

## Webhooks (outbound integrations)

- `list_integrations` / `get_integration_logs` — secrets are never returned.
- `create_integration` / `update_integration` — write; secrets are never returned. Update does not delete.
- `delete_integration` — needs `confirm`. Refused while an AI agent still sends to it.
- `send_leads_to_webhook` — `send_external`. Requires `integrationId`, `leadIds` (max 1000), and `max_leads`. Makes an external call only when `confirm` is true.
- `test_integration` — sample payload; needs `confirm`. External call only when confirm is true.

## CRM connections

- `list_crm_connections` / `get_crm_send_logs` — secrets are never returned.
- `connect_crm` — write. Providers: `zapier` or `groove`. Secrets are never returned.
- `get_crm_auth_url` — read. Returns a URL the **user** must open to finish a browser sign-in. The assistant cannot complete that sign-in. Ask which CRM they use and pass it as `provider`; do not invent provider ids.
- `disconnect_crm` — needs `confirm`. Refused while an AI agent still sends to it, and for a marketplace location.
- `send_leads_to_crm` — `send_external`. Requires `connectionId`, `leadIds` (max **500**), and `max_leads`. External call only when `confirm` is true.
- `test_crm_connection` — needs `confirm`. External call only when confirm is true.

There is no field sync, no key reveal, and no sub-account create on this connection. A Send to CRM agent step also needs an existing connection in this account.

Always dry-run send tools unless the user confirmed a `max_leads` cap.
