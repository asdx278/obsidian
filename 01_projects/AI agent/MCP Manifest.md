---
class: note
area: professional
tags:
  - ai
created: 2025-10-24
---
**MOC:[[MOC - Professional]]**

# MCP Manifest

> [!tldr] ai review
> 

```json
{
  "mcp_version": "1.0",
  "name": "tickets_connector",
  "description": "Доступ к базе решённых тикетов ЭДО",
  "permissions": {
    "read": ["resolved_tickets", "knowledge_articles"],
    "write": []
  },
  "tools": {
    "search_ticket": {
      "input_schema": {
        "type": "object",
        "properties": { "query": { "type": "string" } }
      },
      "output_schema": {
        "type": "object",
        "properties": {
          "ticket_id": { "type": "string" },
          "summary": { "type": "string" },
          "resolution": { "type": "string" }
        }
      }
    }
  }
}

```

### Additional materials