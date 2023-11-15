# Recurring Expenses

## Recurring Expenses Object
Attribute        | Type   | Description
---------        | ----   | -----------
id               | number | Unique identifier for recurring expense
start_date       | string | Denotes when recurring expense starts occurring in ISO 8601 format. If null, then this recurring expense will show up for all time before end_date
end_date         | string | Denotes when recurring expense stops occurring in ISO 8601 format. If null, then this recurring expense has no set end date and will show up for all months after start_date
cadence          | string | One of:<br> <ul> <li>monthly</li> <li>twice a month</li> <li>once a week</li> <li>every 3 months</li> <li>every 4 months</li> <li>twice a year</li> <li>yearly</li></ul>
payee            | string | Payee of the recurring expense
amount           | number | Amount of the recurring expense in numeric format to 4 decimal places
currency         | string | Three-letter lowercase currency code for the recurring expense in ISO 4217 format
description      | string | If any, represents the user-entered description of the recurring expense
billing_date     | string | Expected billing date for this recurring expense for this month in ISO 8601 format
type             | string | This can be one of two values: <br><ul> <li>cleared: The recurring expense has been reviewed by the user</li> <li>suggested: The recurring expense is suggested by the system; the user has yet to review/clear it</li> </ul>
original_name    | string | If any, represents the original name of the recurring expense as denoted by the transaction that triggered its creation
source           | string | This can be one of three values:<br> <ul> <li>manual: User created this recurring expense manually from the Recurring Expenses page</li> <li>transaction: User created this by converting a transaction from the Transactions page</li> <li>system: Recurring expense was created by the system on transaction import</li> <li>Some older recurring expenses may not have a source.</li> </ul>
plaid_account_id | number | If any, denotes the plaid account associated with the creation of this recurring expense (see Plaid Accounts)
asset_id         | number | If any, denotes the manually-managed account (i.e. asset) associated with the creation of this recurring expense (see Assets)
transaction_id   | number | If any, denotes the unique identifier for the associated transaction matching this recurring expense for the current time period
category_id      | number | If any, denotes the unique identifier for the associated category to this recurring expense

## Get Recurring Expenses

Use this endpoint to retrieve a list of recurring expenses to expect for a specified period.

Every month, a different set of recurring expenses is expected. This is because recurring expenses can be once a year, twice a year, every 4 months, etc.

If a recurring expense is listed as “twice a month”, then that recurring expense will be returned twice, each with a different billing date based on when the system believes that recurring expense transaction is to be expected. If the recurring expense is listed as “once a week”, then that recurring expense will be returned in this list as many times as there are weeks for the specified month.

In the same vein, if a recurring expense that began last month is set to “Every 3 months”, then that recurring expense will not show up in the results for this month.

> Example 200 Response
>
> Returns a list of Recurring Expense objects


```json
{
  "recurring_expenses": [
    {
      "id": 264,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "twice a month",
      "payee": "Test 5",
      "amount": "-122.0000",
      "currency": "cad",
      "created_at": "2020-01-30T07:58:43.944Z",
      "description": null,
      "billing_date": "2020-01-01",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    },
    {
      "id": 262,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "monthly",
      "payee": "Test 2",
      "amount": "-32.4500",
      "currency": "usd",
      "created_at": "2020-01-30T07:58:43.921Z",
      "description": "Test description 2",
      "billing_date": "2020-01-03",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    },
    {
      "id": 264,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "twice a month",
      "payee": "Test 5",
      "amount": "-122.0000",
      "currency": "cad",
      "created_at": "2020-01-30T07:58:43.944Z",
      "description": null,
      "billing_date": "2020-01-15",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    }
  ]
}
```

> Example 404 Response

```json
{ "error": "Invalid start_date. Must be in format YYYY-MM-DD" }
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/recurring_expenses`

Parameter         | Type    | Required | Default                        | Description
---------         | ----    | -------- | -------                        | -----------
start_date        | string  | false    | First day of the current month | Accepts a string in ISO-8601 short format. Whatever your start date, the system will automatically return recurring expenses expected for that month. For instance, if you input 2020-01-25, the system will return recurring expenses which are to be expected between 2020-01-01 to 2020-01-31.
debit_as_negative | boolean | false    | false                          | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts.

---
