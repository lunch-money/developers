# Tags

## Tags Object
Attribute Name      | Type    | Description
------------------- | ----    | -----------
id                  | number  | Unique identifier for tag
name                | string  | User-defined name of tag
description         | string  | User-defined description of tag

## Get All Tags
Use this endpoint to get a list of all tags associated with the user's account.

```json
[
    {
        "id": 1807,
        "name": "Wedding",
        "description": "All wedding-related expenses"
    },
    {
        "id": 1808,
        "name": "Honeymoon",
        "description": "All honeymoon-related expenses"
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/tags`

---
