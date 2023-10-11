# Tags

## Tags Object
Attribute Name      | Type    | Description
------------------- | ----    | -----------
id                  | number  | Unique identifier for tag
name                | string  | User-defined name of tag
description         | string  | User-defined description of tag
archived            | boolean | If true, the tag will not show up when creating or updating transactions in the Lunch Money app

## Get All Tags
Use this endpoint to get a list of all tags associated with the user's account.

```json
[
    {
        "id": 1807,
        "name": "Wedding",
        "description": "All wedding-related expenses",
        "archived": false
    },
    {
        "id": 1808,
        "name": "Honeymoon",
        "description": "All honeymoon-related expenses",
        "archived": true
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/tags`

---
