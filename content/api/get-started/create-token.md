---
title: Create API token
pcx_content_type: how-to
weight: 11
---

# Create an API token

{{<Aside type="note" header="Prerequisite">}}
 
Before you begin, [get your zone and account IDs](/fundamentals/get-started/basic-tasks/find-account-and-zone-ids/).
 
{{</Aside>}}

1. From the [Cloudflare dashboard](https://dash.cloudflare.com/profile/api-tokens), go to **My Profile** > **API Tokens**.
2. Select **Create Token**.
3. Select a template from the available [API token templates](/api/reference/template) or create a custom token. We use the **Edit zone DNS** template in the following examples.
4. Add or edit the token name to describe why or how the token is used. Templates are prefilled with a token name and permissions.

![Token template overview screen](/api/static/template-customize.png)

5. Modify the token's permissions. After selecting a permissions group (Account, User, Zone), choose what level of access to grant the token. Most groups offer `Edit` or `Read` options. `Edit` is full CRUDL (create, read, update, delete, list) access, while `Read` is just the read permission and list where appropriate. Refer to the [available token permissions](/api/reference/permissions) for more information.
6. Select which resources the token can take authorized access against. For example, granting `Zone DNS Read` access to a zone `example.com` will allow the token to read DNS records for only that specific zone. Any other zone will return an error for DNS record reads operations. Any other operation on that zone will also return an error.
7. (Optional) Restrict how a token is used in the **Client IP Address Filtering** and **TTL (time to live)** fields.
8. Select `Continue to summary`.
9. Review the token summary. Select **Edit token** to make adjustments. You can also edit a token after creation.

![Token summary screen displaying the resources and permissions selected](/api/static/token-summary.png)

10. Select **Create Token** to generate the token's secret.
11. Copy the secret to a secure place.

  {{<Aside type="warning" header="Warning">}}
 
  The token secret is **only shown once**. Do not store the secret in plaintext where others can to access it. Anyone with this token can perform the authorized actions against the resources the token has been granted access to.
  {{</Aside>}}

![Token creation completion screen displaying your API token and the `curl`command to test your token](/api/static/token-complete.png)

This screen also includes an example command to test the token. The `/user/tokens/verify` endpoint allows fetching the current status of the given token.

```bash
 curl -X GET "https://api.cloudflare.com/client/v4/user/tokens/verify" \
     -H "Authorization: Bearer <token secret>" \
     -H "Content-Type:application/json"
```

The result:

```json
{
  "result": {
    "id": "100bf38cc8393103870917dd535e0628",
    "status": "active"
  },
  "success": true,
  "errors": [],
  "messages": [
    {
      "code": 10000,
      "message": "This API Token is valid and active",
      "type": null
    }
  ]
}
```

With this you have successfully created an API token and can start working with the Cloudflare API. After creating your first API token, you can create API tokens [via the API](/api/how-to/create-via-api).