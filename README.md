# Teetrack MCP Server

> Privacy-first time tracking for freelancers. EU-hosted, with an MCP server for AI assistants.

Teetrack's remote [MCP](https://modelcontextprotocol.io) (Model Context Protocol) server lets AI assistants like Claude, ChatGPT, and Microsoft Copilot operate your time tracker in chat: start and stop timers, log entries, manage projects and clients, review calendar-imported drafts, and answer questions like *"how many billable hours did I log for Acme this month?"*

**Connect URL:**

```
https://teetrack.it/api/mcp
```

No install, no API keys, no local process — it's a hosted server (Streamable HTTP) with OAuth. Point your MCP client at the URL above and log in once.

- Website: https://teetrack.it
- Setup guides: [Claude](https://teetrack.it/guides/mcp/claude) · [ChatGPT](https://teetrack.it/guides/mcp/chatgpt) · [Copilot](https://teetrack.it/guides/mcp/copilot)

## Quick start

### claude.ai / Claude Desktop

1. **Settings → Integrations → Add custom integration**
2. Paste `https://teetrack.it/api/mcp`
3. A browser window opens with a normal Teetrack login — approve once, and the connection persists.

Full walkthrough: https://teetrack.it/guides/mcp/claude

### ChatGPT

ChatGPT connects via its connectors/apps settings using the same URL. Step-by-step: https://teetrack.it/guides/mcp/chatgpt

### Microsoft Copilot

Copilot connects using the same URL. Step-by-step: https://teetrack.it/guides/mcp/copilot

### Any other MCP client

Any client that supports remote MCP servers with OAuth can connect to `https://teetrack.it/api/mcp`. You need a Teetrack account — the free plan works.

## Authentication

The first connection opens a standard browser login to your Teetrack account (OAuth). You approve it **once**; after that the client holds its own authorized connection to your account. No tokens to copy, nothing stored in config files. The tools a client sees are filtered by the scopes granted during that OAuth approval, so some connections may expose a subset of the full list below.

## Tools (35)

| Group | Count | Tools |
|---|---|---|
| Timers | 3 | `start_timer`, `stop_timer`, `get_running_timer` |
| Time Entries | 5 | `list_tracks`, `get_track`, `log_time`, `update_track`, `delete_track` |
| Projects | 5 | `list_projects`, `get_project`, `create_project`, `update_project`, `delete_project` |
| Tags | 4 | `list_tags`, `create_tag`, `update_tag`, `delete_tag` |
| Clients | 5 | `list_clients`, `get_client`, `create_client`, `update_client`, `delete_client` |
| Summaries | 3 | `get_time_summary`, `get_project_summary`, `get_budget_status` |
| Settings | 3 | `get_user_settings`, `update_user_settings`, `get_subscription_usage` |
| Calendar | 7 | `list_calendar_connections`, `list_calendar_events`, `get_calendar_suggestions`, `confirm_draft_track`, `bulk_confirm_draft_tracks`, `dismiss_draft_track`, `trigger_calendar_sync` |

### Timers

| Tool | Description |
|---|---|
| `start_timer` | Start a new timer (creates a track with no end time). Multiple timers can run concurrently. Returns the new running track. |
| `stop_timer` | Stop a running timer by setting its end time to now. If no id is given, stops the most recently started timer. Returns the stopped track. |
| `get_running_timer` | Get all currently running timers (tracks with no end time). Returns an array (empty if none active). |

### Time Entries

| Tool | Description |
|---|---|
| `list_tracks` | List time tracking entries with optional filters and pagination. Returns tracks sorted by start time (newest first). |
| `get_track` | Get a single time tracking entry by its ID. |
| `log_time` | Log a completed time entry with both start and end times. |
| `update_track` | Update an existing time tracking entry. Only provided fields are changed. |
| `delete_track` | Delete a time tracking entry by ID. |

### Projects

| Tool | Description |
|---|---|
| `list_projects` | List all projects with optional pagination. |
| `get_project` | Get a single project by its ID, including billing and budget configuration. |
| `create_project` | Create a new project. |
| `update_project` | Update an existing project. Only provided fields are changed. |
| `delete_project` | Delete a project by ID. |

### Tags

| Tool | Description |
|---|---|
| `list_tags` | List all tags with their track counts and total tracked seconds. |
| `create_tag` | Create a new tag. |
| `update_tag` | Update an existing tag. Only provided fields are changed. |
| `delete_tag` | Delete a tag by ID. |

### Clients

| Tool | Description |
|---|---|
| `list_clients` | List all clients. |
| `get_client` | Get a single client by ID. |
| `create_client` | Create a new client. |
| `update_client` | Update an existing client. Only provided fields are changed. |
| `delete_client` | Delete a client by ID. |

### Summaries

| Tool | Description |
|---|---|
| `get_time_summary` | Get total tracked time broken down by: all time, this year, this month, and today. Returns seconds for each period. |
| `get_project_summary` | Get top projects with hours, billable amounts, and day counts. Useful for understanding where time is being spent. |
| `get_budget_status` | Get budget status for all projects with budgets configured. Shows tracked vs. budgeted hours/amounts and whether over budget. |

### Settings

| Tool | Description |
|---|---|
| `get_user_settings` | Get the current user's display and rounding settings. |
| `update_user_settings` | Update user settings. Only provided fields are changed. |
| `get_subscription_usage` | Get current subscription tier info, feature list, limits, and usage counts. |

### Calendar

| Tool | Description |
|---|---|
| `list_calendar_connections` | List all connected calendar accounts (Google, Outlook, iCal). Returns provider, account email, sync status, and auto-import settings. |
| `list_calendar_events` | List calendar events within a date range from all enabled calendar connections. Excludes all-day events and cancelled events. |
| `get_calendar_suggestions` | List all pending draft time entries imported from calendar events. Returns drafts with their current project assignment for review. |
| `confirm_draft_track` | Confirm a draft time entry imported from a calendar event. Sets draft=false, making it count toward summaries. Use `update_track` first to change description or project. |
| `bulk_confirm_draft_tracks` | Confirm multiple draft time entries at once. Returns the count of confirmed entries. |
| `dismiss_draft_track` | Dismiss a draft time entry imported from a calendar event. The event will not be re-imported in future syncs. |
| `trigger_calendar_sync` | Trigger a calendar sync. If connection_id is provided, syncs that specific connection. Otherwise syncs all enabled connections. Rate limited: once per connection per 5 minutes. |

## Example prompts

- *"Start a timer for the Acme website redesign — homepage copy."*
- *"Stop the timer, and log 2 hours for yesterday afternoon on the same project."*
- *"How many billable hours did I log for Acme this month, and how much of the retainer budget is left?"*
- *"Show me my pending calendar drafts and confirm everything that's a client meeting."*
- *"Which project ate most of my week?"*

## About Teetrack

Teetrack is single-user time tracking and billing for freelancers and consultants. Track hours per client and project, monitor budgets, export PDF/CSV timesheets. EU-hosted (Hetzner), GDPR-first, no screenshots or surveillance. Free plan, €4/mo Business.

Every plan is single-user — one seat, one flat price. Built and run by one person; every support request is answered by the person who wrote the code.

**Get started:** https://teetrack.it
