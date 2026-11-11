---
name: leadfndr-ai-agents
description: Create a Lead Fndr AI agent as a draft, add triggers and actions, set it live, and run one page under max_cost.
---

# AI agents

Use this when the user wants to build, edit, go live, or run a Lead Fndr AI agent.

## Read

`list_agents`, `get_agent`, `list_shared_agents`, `list_agent_folders`, `list_agent_actions`. Canvas coordinates are omitted; actions are a logical graph.

## Create and edit (write)

- `create_agent` — draft only (`name` required). Cannot set it live from here.
- `update_agent` — cannot set it live; does not delete.
- `duplicate_agent` / `use_shared_agent` — copy as a draft.
- `add_agent_trigger` / `update_agent_trigger` / `set_trigger_status` — keyword and schedule. Status is `draft`, `paused`, or `active`.
- `add_agent_action` / `update_agent_action` / `remove_agent_action` — graph edits, not an account delete. Adding an action does not charge. Action kinds include Filter, Enrich / Get Phone Numbers, Skills / AI content, Send to CRM, If/Else, label, tag, similar, competitors, and suppliers. Send to CRM needs an existing connection in this account.
- Folders: `create_folder`, `update_folder`, `move_agent_to_folder`. `delete_folder` unfiles agents and needs `confirm`; it does not delete the agents.

## Go live (write, no charge)

Use `set_agent_status` with `status`: `draft`, `paused`, or `active`. Live needs a search. Do not pass `active: true` to `create_agent` or `update_agent` — that would start an uncapped first page.

Later scheduled pages use the client's wallet. `delete_agent` and `delete_agent_trigger` need `confirm`. Deleting the last trigger drops the agent to draft.

## Run (spend)

`run_agent` (every trigger) and `run_agent_trigger` (one trigger) run **one page** of results. They require `max_cost` and are dry runs without `confirm: true`. The estimate is `resultCount` times the search type's save price, then one page under `max_cost`.
