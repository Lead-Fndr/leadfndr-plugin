---
name: leadfndr-skill-outreach
description: List or edit Lead Fndr Skills and generate outreach for leads with confirm and max_generations.
---

# Skill outreach

Use this when the user wants to write or generate Skill output (intros, summaries, and other outreach) for leads.

## Read

- `list_skills` — custom Skills in this account.
- `list_skill_library` — shared library Skills this account can use.
- `list_skill_types` / `list_skill_variables` — types and template variables for a lead type.
- `get_default_skill_prompt` — built-in default prompt for a lead type and Skill type.
- `get_business_profile` — the business profile Skills use.

The knowledge base and in-app AI Search chat are not separate tools on this connection.

## Edit (write)

`create_skill`, `update_skill`, `clone_skill`, `use_skill_library` (activate or deactivate a library Skill), `update_business_profile`. `delete_skill` needs `confirm`.

## Generate (write, daily quota)

`generate_skill` (one lead) and `bulk_generate_skill` (up to 500 leads) count against the **daily generation quota**, not the wallet. Both require `max_generations`.

Without `confirm: true`, the call is a dry run and generates nothing. With confirm, do not exceed `max_generations`. Pass `leadId` or `leadIds`, and optionally `type` and `promptId`.
