{
  "type": "object",
  "required": ["layout"],
  "properties": {
    "layout": { "type": "object", "properties": { "sections": { "type": "array" } }, "additionalProperties": true },
    "theme_builder": { "type": "object", "additionalProperties": true },
    "global_presets": { "type": "object", "additionalProperties": true },
    "asset_manifest": { "type": "array", "items": { "type": "object" } },
    "notes": { "type": "string" }
  },
  "additionalProperties": false
}
