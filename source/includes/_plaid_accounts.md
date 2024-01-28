# Plaid Accounts

## Plaid Accounts Object

Attribute Name                | Type   | Nullable | Description
----------------------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------
id                            | number | false    | Unique identifier of Plaid account
date_linked                   | string | false    | Date account was first linked in ISO 8601 format
name                          | string | false    | Name of the account. Can be overridden by the user. Field is originally set by Plaid
display_name                  | string | false    | Display name of the account, if not set it will return an empty string.
type                          | string | false    | Primary type of account. Typically one of:<br><ul> <li>credit</li> <li>depository</li> <li>brokerage</li> <li>cash</li> <li>loan</li> <li>Investment</li><ul><br> This field is set by Plaid and cannot be altered
subtype                       | string | true     | Optional subtype name of account. This field is set by Plaid and cannot be altered
mask                          | string | false    | Mask (last 3 to 4 digits of account) of account. This field is set by Plaid and cannot be altered
institution_name              | string | false    | Name of institution associated with account. This field is set by Plaid and cannot be altered
status                        | string | false    | Denotes the current status of the account within Lunch Money. Must be one of:<br><ul> <li>active: Account is active and in good state</li> <li>inactive: Account marked inactive from user. No transactions fetched or balance update for this account.</li> <li>relink: Account needs to be relinked with Plaid.</li> <li>syncing: Account is awaiting first import of transactions</li> <li>error: Account is in error with Plaid</li> <li>not found: Account is in error with Plaid</li> <li>not supported: Account is in error with Plaid</li><ul>
balance                       | string | false    | Current balance of the account in numeric format to 4 decimal places. This field is set by Plaid and cannot be altered
currency                      | string | false    | Currency of account balance in ISO 4217 format. This field is set by Plaid and cannot be altered
balance_last_update           | string | false    | Date balance was last updated in ISO 8601 extended format. This field is set by Plaid and cannot be altered
limit                         | number | true     | Optional credit limit of the account. This field is set by Plaid and cannot be altered
import_start_date             | string | true     | Date of earliest date allowed for importing transactions. Transactions earlier than this date are not imported.
last_import                   | string | true     | Timestamp in ISO 8601 extended format of the last time Lunch Money imported new data from Plaid for this account.
last_fetch                    | string | true     | Timestamp in ISO 8601 extended format of the last successful check from Lunch Money for updated data or timestamps from Plaid in ISO 8601 extended format (not necessarily date of last successful import)
plaid_last_successful_update  | string | false    | Timestamp in ISO 8601 extended format of the last time Plaid successfully connected with institution for new transaction updates, regardless of whether any new data was available in the update.

## Get All Plaid Accounts

Use this endpoint to get a list of all synced Plaid accounts associated with the user's account.

> Example 200 Response

```json
{
  "plaid_accounts": [
    {
      "id": 91,
      "date_linked": "2020-01-28",
      "name": "401k",
      "display_name": "",
      "type": "brokerage",
      "subtype": "401k",
      "mask": "7468",
      "institution_name": "Vanguard",
      "status": "inactive",
      "limit": null,
      "balance": "12345.6700",
      "currency": "usd",
      "balance_last_update": "2020-01-27T01:38:11.862Z",
      "import_start_date": "2023-01-01",
      "last_import": "2019-09-04T12:57:09.190Z",
      "last_fetch": "2020-01-28T01:38:11.862Z",
      "plaid_last_successful_update": "2020-01-27T01:38:11.862Z",
    },
    {
      "id": 89,
      "date_linked": "2020-01-28",
      "name": "Freedom",
      "display_name": "Freedom Card",
      "type": "credit",
      "subtype": "credit card",
      "mask": "1973",
      "institution_name": "Chase",
      "status": "active",
      "limit": 15000,
      "balance": "0.0000",
      "currency": "usd",
      "balance_last_update": "2023-01-27T01:38:07.460Z",
      "import_start_date": "2023-01-01",
      "last_import": "2023-01-24T12:57:03.250Z",
      "last_fetch": "2023-01-28T01:38:11.862Z",
      "plaid_last_successful_update": "2023-01-27T01:38:11.862Z",
    }
  ]
}
```

Plaid Accounts are individual bank accounts that you have linked to Lunch Money via Plaid. You may link one bank but one bank might contain 4 accounts. Each of these accounts is a Plaid Account.

### HTTP Request

`GET https://dev.lunchmoney.app/v1/plaid_accounts`

## Trigger Fetch from Plaid

_This is an experimental endpoint and parameters and/or response may change._

Use this endpoint to trigger a fetch for latest data from Plaid.

> Example 200 Response

```json
true
```

Returns true if there were eligible Plaid accounts to trigger a fetch for. Eligible accounts are those who `last_fetch` value is over 1 minute ago. (Although the limit is every minute, please use this endpoint sparingly!)

Note that fetching from Plaid is a background job. This endpoint simply queues up the job. You may track the `plaid_last_successful_update`, `last_fetch` and `last_import` properties to verify the results of the fetch.

### Body Parameters

Parameter        | Type   | Required | Default | Description
---------------- | ------ | -------- | ------- | -----------------------------------------------------------------------
start_date       | string | false    | -       | Start date for fetch (ignored if end_date is null)
end_date         | string | false    | -       | End date for fetch (ignored if start_date is null)
plaid_account_id | number | false    | -       | Specific ID of a plaid account to fetch. If left empty, endpoint will trigger a fetch for all eligible accounts

### HTTP Request

`POST https://dev.lunchmoney.app/v1/plaid_accounts/fetch`

---
