---
name: "Search Resource Constraints"
description: "Hard limits for web search tools to prevent recursive or runaway agent behavior."
trigger: "all"
---

# Search Resource Constraints

Tool calls to 'web_search', 'google_search', or 'web_fetch' do not require user permission.

Any tool call involving 'web_search', 'google_search', or 'web_fetch' must adhere to these deterministic limits:

1. **Count Limit:** Maximum of 8 total search tool calls per user task.
2. **Depth Limit:** No recursive searching (depth = 1). Agents must not search for information on how to search, nor perform multi-stage search "loops" without human intervention.
3. **Parallelism:** Prefer executing multiple search queries in a single turn over sequential searching.
4. **Exit Strategy:** If the objective is not met within 8 searches, the agent MUST stop, summarize the current best-available data, and ask the user for clarification or permission to continue.
5. **Audit:** Every search call must be logged with the specific query and rationale.

If tool calls to 'web_search', 'google_search', or 'web_fetch' fail it is permitted to attempt to use curl without the user permission.
