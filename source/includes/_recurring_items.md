# Recurring Items

## Recurring Items Object

| Attribute        | Type   | Nullable | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id               | number | false    | Unique identifier for recurring item                                                                                                                                                                                                                                                                                                                                                        |
| start_date       | string | true     | Denotes when recurring item starts occurring in ISO 8601 format. If null, then this recurring item will show up for all time before end_date                                                                                                                                                                                                                                             |
| end_date         | string | true     | Denotes when recurring item stops occurring in ISO 8601 format. If null, then this recurring item has no set end date and will show up for all months after start_date                                                                                                                                                                                                                   |
| cadence          | string | false    | One of:<br> <ul> <li>monthly</li> <li>twice a month</li> <li>once a week</li> <li>every 3 months</li> <li>every 4 months</li> <li>twice a year</li> <li>yearly</li></ul>                                                                                                                                                                                                                       |
| payee            | string | false    | Payee or payer of the recurring item                                                                                                                                                                                                                                                                                                                                                                 |
| amount           | number | false    | Amount of the recurring item in numeric format to 4 decimal places.  For recurring items with flexible amounts, this is the average of the specified min and max amounts.                                                                                                                                                                                                                                                                                                                          |
| currency         | string | false    | Three-letter lowercase currency code for the recurring item in ISO 4217 format                                                                                                                                                                                                                                                                                                              |
| created_at       | string | false    | The date and time of when the recurring item was created (in the ISO 8601 extended format).                                                                                                                                                                                                                                                                                                 |
| updated_at       | string | false    | The date and time of when the recurring item was updated (in the ISO 8601 extended format).                                                                                                                                                                                                                                                                                                 |
| billing_date     | string | false    | Expected transaction date for this recurring item for this month in ISO 8601 format                                                                                                                                                                                                                                                                                                             |
| type             | string | false    | This can be one of two values: <br><ul> <li>cleared: The recurring item has been reviewed by the user</li> <li>suggested: The recurring item is suggested by the system; the user has yet to review/clear it</li> </ul>                                                                                                                                                                  |
| original_name    | string | true     | If any, represents the original name of the recurring item as denoted by the transaction that triggered its creation                                                                                                                                                                                                                                                                        |
| description      | string | true     | If any, represents the user-entered description of the recurring item                                                                                                                                                                                                                                                                                                                       |
| plaid_account_id | number | true     | If any, denotes the plaid account associated with the creation of this recurring item (see Plaid Accounts)                                                                                                                                                                                                                                                                                  |
| asset_id         | number | true     | If any, denotes the manually-managed account (i.e. asset) associated with the creation of this recurring item (see Assets)                                                                                                                                                                                                                                                                  |
| source           | string | false    | This can be one of three values:<br> <ul> <li>manual: User created this recurring item manually from the Recurring Items page</li> <li>transaction: User created this by converting a transaction from the Transactions page</li> <li>system: Recurring item was created by the system on transaction import</li></ul> Some older recurring items may not have a source.  |
| notes            | string | true     | If any, the user-entered notes for the recurring item                                                                                                                                                                                      |
| category_id      | number | true     | If any, denotes the unique identifier for the associated category to this recurring item                                                                                                                                                                                                                                                                                                    |
| category_group_id| number | true     | If any, denotes the unique identifier of associated category group                                                                                                                                                                                                                                                          |
| is_income        | boolean| false    | Based on the associated category's property, denotes if the recurring transaction is treated as income                                                                                                                                                                                                                        |
| exclude_from_totals | boolean | false| Based on the associated category's property, denotes if the recurring transaction is excluded from totals                                                                                                                                                                                                                     |
| granularity      | string | false    | The unit of time used to define the cadence <br>One of:<br> <ul> <li>weeks</li> <li>months</li> <li>years</li><ul>                                                                                                                                                                                                                     |
| quantity         | number | true     | The number of granularity units between each recurrence                                                                                                                                                                                                       |
| occurrences      | object | false     | TO BE DEFINED - this appears to be an object with dates as keys and empty arrays as values.  Not sure why it can't be an array of date strings.  It also isn't clear if this is bounded by anything (ie: current month plus one previous and after?)                                                                                                                                                                                                             |
| transactions_within_range  | list | true     | TO BE DEFINED OR KEPT PRIVATE                                                                                                                                                                                                 |
| missing_dates_within_range | list  | true    | TO BE DEFINED - is this for the transactions not found for the dates in occurrences? If so, don't we get this via the empty array?                                                                                             |
| date             | string | true     | Denotes the value of the `start_date` query parameter, or if none was provided, the date when the request was made.  This indicates the month used by the system when populating the response.                                                                                                                                                                                      |
| to_base          | number | false    | The amount converted to the user's primary currency. If the multicurrency feature is not being used, to_base and amount will be the same.                                                                                                                                                                       |

## Get Recurring Items

Use this endpoint to retrieve a list of recurring items to expect for a specified period.

A different set of recurring items is expected every month. These can be once a year, twice a year, every four months, etc.

If a recurring item is listed as “twice a month,” then the recurring item object returned will have an `occurrences` attribute populated by the different billing dates the system believes recurring transactions should occur, including the two dates in the current month, the last transaction date prior to the month, and the next transaction date after the month. 

If the recurring item is listed as “once a week,” then the recurring item object returned will have an `occurrences` object populated with as many times as there are weeks for the specified month, along with the last transaction from the previous month and the next transaction for the next month.

In the same vein, if a recurring item that began last month is set to “Every 3 months”, then that recurring item object that occurred will not include any dates for this month.

> Example 200 Response
>
> Returns a list of Recurring Item objects

```json
[
    {
        "id": 940537,
        "start_date": null,
        "end_date": null,
        "cadence": "once a week",
        "payee": "Weekly Income",
        "amount": "-200.0000",
        "currency": "usd",
        "created_at": "2024-05-27T12:48:33.777Z",
        "updated_at": "2024-05-27T12:48:33.777Z",
        "billing_date": "2024-05-01",
        "type": "cleared",
        "original_name": null,
        "description": null,
        "plaid_account_id": null,
        "asset_id": null,
        "source": "manual",
        "notes": null,
        "category_id": 911979,
        "category_group_id": null,
        "is_income": true,
        "exclude_from_totals": false,
        "granularity": "weeks",
        "quantity": 1,
        "occurrences": {
            "2024-05-29": [],
            "2024-06-05": [],
            "2024-06-12": [],
            "2024-06-19": [],
            "2024-06-26": [],
            "2024-07-03": []
        },
        "transactions_within_range": [],
        "missing_dates_within_range": [
            "2024-06-05",
            "2024-06-12",
            "2024-06-19",
            "2024-06-26"
        ],
        "date": "2024-06-04",
        "to_base": -200
    },
    {
        "id": 838965,
        "start_date": null,
        "end_date": null,
        "cadence": "monthly",
        "payee": "Google Fi",
        "amount": "50.0000",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.698Z",
        "updated_at": "2024-04-05T20:02:22.698Z",
        "billing_date": "2024-01-25",
        "type": "cleared",
        "original_name": null,
        "description": "Cell phone plan",
        "plaid_account_id": null,
        "asset_id": 109419,
        "source": "manual",
        "notes": null,
        "category_id": 911972,
        "category_group_id": null,
        "is_income": false,
        "exclude_from_totals": false,
        "granularity": "months",
        "quantity": 1,
        "occurrences": {
            "2024-05-25": [],
            "2024-06-25": [],
            "2024-07-25": []
        },
        "transactions_within_range": [],
        "missing_dates_within_range": [
            "2024-06-25"
        ],
        "date": "2024-06-04",
        "to_base": 50
    },
    {
        "id": 838964,
        "start_date": null,
        "end_date": null,
        "cadence": "monthly",
        "payee": "Direct Deposit",
        "amount": "4500.0000",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.477Z",
        "updated_at": "2024-04-05T20:02:22.477Z",
        "billing_date": "2024-01-01",
        "type": "cleared",
        "original_name": null,
        "description": "Income",
        "plaid_account_id": null,
        "asset_id": 109421,
        "source": "manual",
        "notes": null,
        "category_id": 911979,
        "category_group_id": null,
        "is_income": true,
        "exclude_from_totals": false,
        "granularity": "months",
        "quantity": 1,
        "occurrences": {
            "2024-06-01": [],
            "2024-07-01": []
        },
        "transactions_within_range": [],
        "missing_dates_within_range": [
            "2024-06-01"
        ],
        "date": "2024-06-04",
        "to_base": 4500
    },
    {
        "id": 838963,
        "start_date": null,
        "end_date": null,
        "cadence": "monthly",
        "payee": "Geico",
        "amount": "145.0000",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.244Z",
        "updated_at": "2024-04-05T20:02:22.244Z",
        "billing_date": "2024-01-01",
        "type": "cleared",
        "original_name": null,
        "description": "Car insurance",
        "plaid_account_id": null,
        "asset_id": 109419,
        "source": "manual",
        "notes": null,
        "category_id": 911972,
        "category_group_id": null,
        "is_income": false,
        "exclude_from_totals": false,
        "granularity": "months",
        "quantity": 1,
        "occurrences": {
            "2024-06-01": [],
            "2024-07-01": []
        },
        "transactions_within_range": [],
        "missing_dates_within_range": [
            "2024-06-01"
        ],
        "date": "2024-06-04",
        "to_base": 145
    },
    {
        "id": 838962,
        "start_date": null,
        "end_date": null,
        "cadence": "monthly",
        "payee": "Rent",
        "amount": "850.0000",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.022Z",
        "updated_at": "2024-04-05T20:02:22.022Z",
        "billing_date": "2024-01-01",
        "type": "cleared",
        "original_name": null,
        "description": "Monthly rent payable to Mrs Smith",
        "plaid_account_id": null,
        "asset_id": 109421,
        "source": "manual",
        "notes": null,
        "category_id": 911972,
        "category_group_id": null,
        "is_income": false,
        "exclude_from_totals": false,
        "granularity": "months",
        "quantity": 1,
        "occurrences": {
            "2024-06-01": [],
            "2024-07-01": []
        },
        "transactions_within_range": [],
        "missing_dates_within_range": [
            "2024-06-01"
        ],
        "date": "2024-06-04",
        "to_base": 850
    }
]```

> Example 404 Response

```json
{ "error": "Invalid start_date. Must be in format YYYY-MM-DD" }
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/recurring_items`

| Parameter         | Type    | Required | Default                        | Description                                                                                                                                                                                                                                                                                       |
| ----------------- | ------- | -------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| start_date        | string  | false    | First day of the current month | Accepts a string in ISO-8601 short format. Whatever your start date, the system will automatically return recurring items expected for that month. For instance, if you input 2020-01-25, the system will return recurring items which are to be expected between 2020-01-01 to 2020-01-31. |
| debit_as_negative | boolean | false    | false                          | Pass in true if you’d like items to be returned as negative amounts and credits as positive amounts.                                                                                                                                                                                           |

---
