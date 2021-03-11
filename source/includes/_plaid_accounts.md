# Plaid Accounts

## Plaid Accounts Object
Attribute Name      | Type   | Description
------------------- | ----   | -----------
id                  | number | Unique identifier of Plaid account
date_linked         | string | Date account was first linked in ISO 8601 extended format
name                | string | Name of the account. Can be overridden by the user. Field is originally set by Plaid
type                | string | Primary type of account. Typically one of:<br><ul> <li>credit</li> <li>depository</li> <li>brokerage</li> <li>cash</li> <li>loan</li> <li>Investment</li><ul><br> This field is set by Plaid and cannot be altered
subtype             | string | Optional subtype name of account. This field is set by Plaid and cannot be altered
mask                | string | Mask (last 3 to 4 digits of account) of account. This field is set by Plaid and cannot be altered
institution_name    | string | Name of institution associated with account. This field is set by Plaid and cannot be altered
status              | string | Denotes the current status of the account within Lunch Money. Must be one of:<br><ul> <li>active: Account is active and in good state</li> <li>inactive: Account marked inactive from user. No transactions fetched or balance update for this account.</li> <li>relink: Account needs to be relinked with Plaid.</li> <li>syncing: Account is awaiting first import of transactions</li> <li>error: Account is in error with Plaid</li> <li>not found: Account is in error with Plaid</li> <li>not supported: Account is in error with Plaid</li><ul>
last_import         | string | Date of last imported transaction in ISO 8601 extended format (not necessarily date of last attempted import)
balance             | string | Current balance of the account in numeric format to 4 decimal places. This field is set by Plaid and cannot be altered
currency            | string | Currency of account balance. This field is set by Plaid and cannot be altered
balance_last_update | string | Date balance was last updated in ISO 8601 extended format. This field is set by Plaid and cannot be altered
limit               | number | Optional credit limit of the account. This field is set by Plaid and cannot be altered

## Get All Plaid Accounts
Use this endpoint to get a list of all synced Plaid accounts associated with the user's account.

> Example 200 Response

```json
{
  "plaid_accounts": [
    {
      "id": 91,
      "date_linked": "2020-01-28T14:15:09.111Z",
      "name": "401k",
      "type": "brokerage",
      "subtype": "401k",
      "mask": "7468",
      "institution_name": "Vanguard",
      "status": "inactive",
      "last_import": "2019-09-04T12:57:09.190Z",
      "balance": "12345.6700",
      "currency": "usd",
      "balance_last_update": "2020-01-27T01:38:11.862Z",
      "limit": null
    },
    {
      "id": 89,
      "date_linked": "2020-01-28T14:15:09.111Z",
      "name": "Freedom",
      "type": "credit",
      "subtype": "credit card",
      "mask": "1973",
      "institution_name": "Chase",
      "status": "active",
      "last_import": "2019-09-04T12:57:03.250Z",
      "balance": "0.0000",
      "currency": "usd",
      "balance_last_update": "2020-01-27T01:38:07.460Z",
      "limit": 15000
    }
  ]
}
```

Plaid Accounts are individual bank accounts that you have linked to Lunch Money via Plaid. You may link one bank but one bank might contain 4 accounts. Each of these accounts is a Plaid Account.

### HTTP Request

`GET https://dev.lunchmoney.app/v1/plaid_accounts`

---
