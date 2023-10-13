# User

## User Object

| Attribute Name | Type   | Description                                                                             |
| -------------- | ------ | --------------------------------------------------------------------------------------- |
| user_id        | number | Unique identifier for user                                                              |
| user_name      | string | User's' name                                                                            |
| user_email     | string | User's email                                                                            |
| account_id     | number | Unique identifier for the associated budgeting account                                  |
| budget_name    | string | Name of the associated budgeting account                                                |
| api_key_label  | string | User-defined label of the developer API key used. Returns null if nothing has been set. |

## Get User

Use this endpoint to get details on the current user.

```json
{
  "user_name": "User 1",
  "user_email": "user-1@lunchmoney.dev",
  "user_id": 18328,
  "account_id": 18221,
  "budget_name": "🏠 Family budget"
  "api_key_label": "Side project dev key"
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/me`

---
