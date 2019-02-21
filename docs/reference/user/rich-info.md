# Rich info {#RefRichInfo}

_Author: Artyom Kazak_

---

This page describes a part of the user profile called "Rich info". The corresponding feature is called "Rich profiles".

## Summary {#RefRichInfoSummary}

For every team user we can store a list of key-value pairs that are displayed in the user profile. This is similar to "custom profile fields" in Slack and other enterprise messengers.

Different users can have different sets of fields; there is no team-wide schema for fields. All fields are strings.

Only team members and partners can see the user's rich info.

## API {#RefRichInfoApi}

### Querying rich info {#RefRichInfoGet}

`GET /users/:id/rich_info`. Sample output:

```json
{
    "fields": [
        {
            "type": "Department",
            "value": "Sales & Marketing"
        },
        {
            "type": "Favorite color",
            "value": "Blue"
        }
    ]
}
```

### Setting rich info {#RefRichInfoPut}

Not implemented yet.

### Events {#RefRichInfoEvents}

When user's rich info changes, the backend sends out an event to all team members:

```json
{
    "type": "user.rich-info-update",
    "user": {
        "id": "<user ID>"
    }
}
```

Connected users who are not members of user's team will not receive an event (nor can they query user's rich info by other means).

## SCIM support {#RefRichInfoScim}

Rich info can be pushed to Wire by setting the `"richInfo"` field belonging to the `"urn:wire:scim:schemas:profile:1.0"` extension:

```javascript
PUT /scim/v2/Users/:id

{
    ...,
    "urn:wire:scim:schemas:profile:1.0": {
        "richInfo": [
            {
                "type": "Department",
                "value": "Sales & Marketing"
            },
            {
                "type": "Favorite color",
                "value": "Blue"
            }
        ]
    }
}
```

Rich info can not be pulled from Wire and will not show up in SCIM queries.

### SCIM provisioning agent support {#RefRichInfoScimAgents}

* Okta: unable to push fields in the format we require (checked on 2019-02-21).

* OneLogin: likely able to push fields.

## Limitations {#RefRichInfoLimitations}

* JSON-encoded rich info cannot exceed 5000 characters in length. There are no limitations on the number of fields, or the length of field names or values.