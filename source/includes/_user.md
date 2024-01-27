# User

## User Object

| Attribute Name | Type   | Nullable | Description                                                                             |
| -------------- | ------ | -------- | --------------------------------------------------------------------------------------- |
| user_id        | number | false    | Unique identifier for user                                                              |
| user_name      | string | false    | User's' name                                                                            |
| user_email     | string | false    | User's email                                                                            |
| account_id     | number | false    | Unique identifier for the associated budgeting account                                  |
| budget_name    | string | false    | Name of the associated budgeting account                                                |
| api_key_label  | string | true     | User-defined label of the developer API key used. Returns null if nothing has been set. |

## Get User

Use this endpoint to get details on the current user.

```json
{
  "user_name": "User 1",
  "user_email": "user-1@lunchmoney.dev",
  "user_id": 18328,
  "account_id": 18221,
  "budget_name": "🏠 Family budget",
  "api_key_label": "Side project dev key"
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/me`

---
