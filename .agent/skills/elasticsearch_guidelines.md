# Skill: Elasticsearch Template Engineering

This skill file provides the agent with specific domain knowledge for writing Elasticsearch templates.

## 1. Component Template Syntax
When creating a component template, the payload generally looks like:
```json
{
  "template": {
    "mappings": {
      "properties": { ... }
    },
    "settings": { ... }
  }
}
```

## 2. Index Template Syntax
When assembling an index template using components:
```json
{
  "index_patterns": ["logs-*"],
  "composed_of": ["component_name_1", "component_name_2"],
  "priority": 200,
  "template": {
    "aliases": {
      "logs_write": {}
    }
  }
}
```

## 3. Best Practices
- Use `keyword` for exact matches, aggregation, and sorting.
- Use `text` only when full-text search is required.
- Consider `ignore_above: 256` for keyword fields generated automatically.
- Map `date` fields appropriately (e.g., `"format": "strict_date_optional_time||epoch_millis"`).
