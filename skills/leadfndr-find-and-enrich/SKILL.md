---
name: leadfndr-find-and-enrich
description: Preview leads for free, run paid searches, and save or enrich contacts in Lead Fndr with confirm and max_cost.
---

# Find and enrich leads

Use this when the user wants to search, preview, save, or enrich leads.

## Free previews (read, no charge, no save)

| Tool | Notes |
|---|---|
| `search_people` | People by keyword and locations. Does not save. |
| `search_local_businesses` | Requires `name` and `location`. Omit any save id; saving is a separate spend tool. |
| `search_companies` | Companies by keyword. Does not save. |
| `estimate_property_search` | Match count and cost estimate for a property search. Does not charge. |
| `location_autocomplete` | Location suggestions (`q` required). |
| `suggest_filter_values` | Typeahead for an allow-listed filter field. |

Inspect existing data with `list_lists`, `get_list`, `get_list_leads`, `get_list_history`, `get_lead`, `get_lead_activity`, and `list_recent_searches`. `buyer_intent_status` and `verify_website_pixel` are read-only status checks on a list.

## Paid searches (spend)

These save into a list, charge the wallet, and require `max_cost`. Dry-run first unless the user already confirmed a cap.

| Tool | Required besides `max_cost` |
|---|---|
| `search_domain_emails` | `domain`, list id |
| `search_influencers` | list id |
| `search_buyer_intent` | `keyword`, list id. May return early with `still_running: true`; remainder sourcing on this path is disabled. |
| `search_properties` | list id. Prefer `estimate_property_search` first. |

## Save and enrich (spend)

| Tool | Required besides `max_cost` | Notes |
|---|---|---|
| `save_leads` | list id, `leadType` | Saves previewed `results` or a bulk `allLimit` (max 1000). |
| `enrich_lead` | `leadId` | One lead. |
| `bulk_enrich_leads` | `leadIds` | Up to 100 leads. |

Without `confirm: true`, these return an estimate and charge nothing. With confirm, the estimate is recomputed and refused if it exceeds `max_cost`.

Enrich is estimated per lead at the price that lead type is enriched at (people phone, website visitor, influencer, property skip trace). Local and company enrich is free. Buyer-intent leads are not enriched.

A list's active "new lead" automations can run on saved leads. Their estimate is added to the save estimate and held to `max_cost`. They start in the background; their charges are not in the amount already charged.

## Lists and labels (write)

`create_list`, `update_list`, `move_leads`, `create_label`, `update_label`, `apply_label`, `remove_label` need write. `delete_list`, `delete_lead`, and `delete_label` also need `confirm`. `delete_list` is refused for the default list and for a list an AI agent still feeds.
