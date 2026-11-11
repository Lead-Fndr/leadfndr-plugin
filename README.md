# Lead Fndr

<p align="center">
  <img src="assets/logo-horizontal-dark.svg" alt="Lead Fndr" width="360" />
</p>

**AI-powered prospecting.** Find the right people. Start the right conversations.

This plugin connects an AI assistant to your Lead Fndr account over the official remote assistant endpoint. You can look up lists and leads, preview searches, save and enrich contacts (with a spend cap), run AI agents, generate Skill outreach, and send leads to a CRM or webhook you already connected.

## Install

1. Install the **Lead Fndr** plugin. In Claude Code, run:

   ```
   /plugin install leadfndr --marketplace Lead-Fndr/leadfndr-plugin
   ```

2. Create an **Assistant key** in Lead Fndr: open **Integrations → API & Assistant** (`/settings/integration/api`). Start with the **Read** scope.
3. When prompted, paste the key into the **Lead Fndr Assistant key** field. The plugin sends it as `Authorization: Bearer <key>`. The key is never stored in the plugin package.

A sign-in cookie is refused. Every call must use a key.

### Key kinds and scopes

| Kind | What it can do on this connection |
|---|---|
| **Assistant key** (has scopes) | Limited to the scopes you grant. This is the key you should use. |
| **API key** (no scopes) | Read only on this connection. |

Grant only what you need:

| Scope | What it can do |
|---|---|
| `read` | Lists, leads, wallet, dashboard, AI agents, Skills, business profile, CRM connections, and the agency roster. Default for a new Assistant key. |
| `write` | Create or edit lists, leads, labels, agents, Skills, webhooks, and destinations. Generate Skill content (needs confirm + `max_generations`). Deletes and disconnects need `confirm`. Going live uses `set_agent_status`, not `create_agent` / `update_agent`. |
| `spend` | Paid search, save, enrich, and one bounded agent run. Needs `confirm` and `max_cost`. |
| `send_external` | Send to an existing webhook or CRM connection. Needs `confirm` and `max_leads`. |

Write, spend, and send are granted from a signed-in Lead Fndr session, not from another key.

## Safety

Spend, send, and generate calls are dry runs unless you pass `confirm: true` **and** the matching cap:

- Spend tools: `max_cost` (USD this call may charge)
- Send tools: `max_leads`
- Generate tools: `max_generations` (counts against the daily generate quota, not the wallet)

If the recomputed estimate is above the cap, nothing is charged or sent. Deletes and disconnects also need `confirm: true` or they write nothing. Integration and CRM list tools never return secrets.

A list's active "new lead" automations can run on leads this connection saves. Their estimated cost is included in the save estimate and held to `max_cost`. They start in the background after the save, so their charges are not in the amount already charged.

## What you can do

Use the bundled skills, or call the tools directly once the connection is on:

- **Find and enrich leads** — free previews for people, local businesses, and companies; a free property estimate; paid searches that save to a list; `save_leads`, `enrich_lead`, and `bulk_enrich_leads`.
- **AI agents** — create a draft, add triggers and workflow actions, set it live with `set_agent_status`, and run one page with `run_agent` / `run_agent_trigger`.
- **Skill outreach** — list, edit, and generate Skill output for a lead (`generate_skill`, `bulk_generate_skill`).
- **Send leads** — webhooks (`send_leads_to_webhook`) or an existing CRM connection (`send_leads_to_crm`).
- **Sub-accounts** — agency owners can run a call in a paying client's account with `as_sub_account_id`. That uses the client's wallet and daily generate quota. Roster tools stay on the agency book.

## Limits

All keys on an account share these limits. A `429` response includes `Retry-After`.

| What | Limit |
|---|---|
| Calls | 60 per minute |
| Confirmed spend / send / generate calls | 10 per minute |
| Failed keys from one address | 30 per minute, then that address waits until the minute is up |

## Endpoint

```json
{
  "mcpServers": {
    "leadfndr": {
      "type": "http",
      "url": "https://api.leadfndr.com/api/v1/mcp",
      "headers": {
        "Authorization": "Bearer ${LEADFNDR_API_KEY}"
      }
    }
  }
}
```

Product site: [leadfndr.com](https://leadfndr.com)

## License

MIT

## Support

Email support@agencyownersupport.com or visit https://leadfndr.com.
