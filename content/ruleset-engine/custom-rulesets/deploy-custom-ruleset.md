---
pcx_content_type: how-to
title: Deploy a custom ruleset
weight: 4
---

# Deploy a custom ruleset

Before you begin:

1. Obtain the name of the phase where you want to deploy the custom ruleset.
2. [Create a custom ruleset](/ruleset-engine/custom-rulesets/create-custom-ruleset/) and keep the ID of the new custom ruleset.
3. [Fetch the rules already present in the phase entry point ruleset](/ruleset-engine/basic-operations/view-rulesets/#view-the-rules-included-in-a-ruleset). You must include in the `PUT` request all existing rules you want to keep.

To deploy a custom ruleset, add a rule that executes the custom ruleset. Define the rule scope in the rule expression.

{{<Aside type="warning">}}
You must include `AND zone.level eq "ENT"` in the expression of the rule deploying the ruleset or else the API operation will fail.
{{</Aside>}}

## Example

The following `PUT` request adds a rule that executes a custom ruleset when the zone name matches `example.com`.

```json
---
header: Request
---
curl -X PUT \
"https://api.cloudflare.com/client/v4/accounts/<ACCOUNT_ID>/rulesets/phases/http_request_firewall_custom/entrypoint" \
-H "Authorization: Bearer <API_TOKEN>" \
-d '{
  "rules": [
    {
      "action":"execute",
      "description":"Execute custom ruleset",
      "expression": "cf.zone.name == \"example.com\" AND zone.level eq \"ENT\"",
      "action_parameters": {
        "id":"<CUSTOM_RULESET_ID>"
      }
    },
    {
      "id": "<EXISTING_PHASE_RULE_ID_1>"
    },
    {
      "id": "<EXISTING_PHASE_RULE_ID_2>"
    }
  ]
}'
```

The response displays the rules in your phase.

```json
---
header: Response
---
{
  "result": {
    "id": "<ACCOUNT_PHASE_RULESET_ID>",
    "name": "http_request_firewall_custom phase entry point ruleset for my account",
    "description": "Execute several rulesets",
    "kind": "root",
    "version": "3",
    "rules": [
      {
        "id": "<PHASE_RULE_ID>",
        "version": "1",
        "action": "execute",
        "description":"Execute custom ruleset",
        "action_parameters": {
          "id": "<CUSTOM_RULESET_ID>",
          "version": "latest"
        },
        "expression": "cf.zone.name == \"example.com\" AND zone.level eq \"ENT\"",
        "last_updated": "2021-03-18T18:35:14.135697Z",
        "ref": "<PHASE_RULE_REF>",
        "enabled": true
      },
      {
        "id": "<EXISTING_PHASE_RULE_ID_1>",
        "version": "1",
        "action": "execute",
        "action_parameters": {
          "id": "<EXECUTED_RULESET_ID_1>",
          "version": "latest"
        },
        "expression": "cf.zone.name eq  \"example.com\" AND zone.level eq \"ENT\"",
        "last_updated": "2021-03-16T15:51:49.180378Z",
        "ref": "<EXISTING_PHASE_RULE_REF_1>",
        "enabled": true
      },
      {
        "id": "<EXISTING_PHASE_RULE_ID_2>",
        "version": "1",
        "action": "execute",
        "action_parameters": {
          "id": "<EXECUTED_RULESET_ID_2>",
          "version": "latest"
        },
        "expression": "cf.zone.name eq  \"example.com\" AND zone.level eq \"ENT\"",
        "last_updated": "2021-03-16T15:50:29.861157Z",
        "ref": "<EXISTING_PHASE_RULE_REF_2>",
        "enabled": true
      }
    ],
    "last_updated": "2021-03-18T18:35:14.135697Z",
    "phase": "http_request_firewall_custom"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```
