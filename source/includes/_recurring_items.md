# Recurring Items

<aside class="notice">
This API (v1) will not progress to a generally available state. For new projects, we recommend using the <a href="https://alpha.lunchmoney.dev/v2/docs#tag/recurring_items">v2 API</a>, which is in open alpha and actively maintained.
</aside>

## Recurring Items Object

| Attribute        | Type   | Nullable | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id               | number | false    | Unique identifier for recurring item                                                                                                                                                                                                                                                                                                                                                        |
| start_date       | string | true     | Denotes when recurring item starts occurring in ISO 8601 format. If null, then this recurring item will show up for all time before end_date                                                                                                                                                                                                                                             |
| end_date         | string | true     | Denotes when recurring item stops occurring in ISO 8601 format. If null, then this recurring item has no set end date and will show up for all months after start_date                                                                                                                                                                                                                   |
| payee            | string | false    | Payee or payer of the recurring item                                                                                                                                                                                                                                                                                                                                                                 |
| currency         | string | false    | Three-letter lowercase currency code for the recurring item in ISO 4217 format                                                                                                                                                                                                                                                                                                              |
| created_by       | number | false    | The id of the user who created this recurring item.                                                                                                                                                                                                                                                                                                              |
| created_at       | string | false    | The date and time of when the recurring item was created (in the ISO 8601 extended format).                                                                                                                                                                                                                                                                                                 |
| updated_at       | string | false    | The date and time of when the recurring item was updated (in the ISO 8601 extended format).                                                                                                                                                                                                                                                                                                 |
| billing_date     | string | false    | Initial date that a transaction associated with this recurring item occurred.   This date is used in conjunction with values of `quantity` and `granularity` to determine the expected dates of recurring transactions in the period.                                                                                                                                                                                                                                                                                                             |
| original_name    | string | true     | If any, represents the original name of the recurring item as denoted by the transaction that triggered its creation                                                                                                                                                                                                                                                                        |
| description      | string | true     | If any, represents the user-entered description of the recurring item                                                                                                                                                                                                                                                                                                                       |
| plaid_account_id | number | true     | If any, denotes the plaid account associated with the creation of this recurring item (see Plaid Accounts)                                                                                                                                                                                                                                                                                  |
| asset_id         | number | true     | If any, denotes the manually-managed account (i.e. asset) associated with the creation of this recurring item (see Assets)                                                                                                                                                                                                                                                                  |
| source           | string | false    | This can be one of four values:<br> <ul> <li>manual: User created this recurring item manually from the Recurring Items page</li> <li>transaction: User created this by converting a transaction from the Transactions page</li> <li>system: Recurring item was created by the system on transaction import</li> <li> null: Some older recurring items may not have a source.</li></ul>  |
| notes            | string | true     | If any, the user-entered notes for the recurring item                                                                                                                                                                                      |
| amount           | number | false    | Amount of the recurring item in numeric format to 4 decimal places.  For recurring items with flexible amounts, this is the average of the specified min and max amounts.                                                                                                                                                                                                                                                                                                                          |
| category_id      | number | true     | If any, denotes the unique identifier for the associated category to this recurring item                                                                                                                                                                                                                                                                                                    |
| category_group_id| number | true     | If any, denotes the unique identifier of associated category group                                                                                                                                                                                                                                                          |
| is_income        | boolean| false    | Based on the associated category's property, denotes if the recurring transaction is treated as income                                                                                                                                                                                                                        |
| exclude_from_totals | boolean | false| Based on the associated category's property, denotes if the recurring transaction is excluded from totals                                                                                                                                                                                                                     |
| granularity      | string | false    | The unit of time used to define the cadence of the recurring item <br>One of:<br> <ul> <li>day</li> <li>week</li> <li>month</li><li>year</li><ul>                                                                                                                                                                                                                     |
| quantity         | number | true     | The number of granularity units between each recurrence                                                                                                                                                                                                       |
| occurrences      | object | false     | An object which contains dates as keys and lists as values.  The dates will include all the dates in the month that a recurring item is expected, as well as the last date in the previous period and the first date in the next period.  The value for each key is a list of [Summarized Transaction Objects](#summarized-transaction-object) that matched the recurring item for that date (if any)                                                                                                                                          |
| transactions_within_range  | list | true     | A list of all the [Summarized Transaction Objects](#summarized-transaction-object) for transactions that that have occurred in the query month for the recurring item (if any)                                                                                                                                                              |
| missing_dates_within_range | list  | true    | A list of date strings when a recurring transaction is expected but has not (yet) occurred.                                                      |
| date             | string | true     | Denotes the value of the `start_date` query parameter, or if none was provided, the date when the request was made.  This indicates the month used by the system when populating the response.                                                                                                                                                                                      |
| to_base          | number | false    | The amount converted to the user's primary currency. If the multicurrency feature is not being used, to_base and amount will be the same.                                                                                                                                                                       |

## Summarized Transaction Object

This object, which includes a subset of fields from the Transaction Object, may be included in the list of transactions associated with a date in the `occurrences` object, or in the `transactions_within_range` list.

| Attribute        | Type   | Nullable | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id               | number | false    | Unique identifier for the transaction that matched this recurring item                                                                                                                                                                                                                                                                                                                                                        |
| date                       | string        | false    | Date of transaction in ISO 8601 format                                                                                                                                                                                                                                                                          |
| amount                     | string        | false    | Amount of the transaction in numeric format to 4 decimal places                                                                                                                                                                                                                                                 |
| currency                   | string        | false    | Three-letter lowercase currency code of the transaction in ISO 4217 format                                                                                                                                                                                                                                      |
| payee            | string | false    | Payee or payer of the recurring item                                                                                                                                                                                                                                                                                                                                                                 |
| category_id                | number        | true     | Unique identifier of associated category (see Categories)                                                                                                                                                                                                                                                       |
| recurring_id               | number        | true     | Unique identifier of associated recurring item                                                                                                                                                                       |
| to_base                    | number        | false    | The amount converted to the user's primary currency. If the multicurrency feature is not being used, to_base and amount will be the same.                                                                                                                                                                       |

## Get Recurring Items

Use this endpoint to retrieve a list of recurring items to expect for a specified month.

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
        "payee": "Weekly Income",
        "currency": "usd",
        "created_at": "2024-05-27T12:48:33.777Z",
        "updated_at": "2024-05-27T12:48:33.777Z",
        "billing_date": "2024-05-01",
        "original_name": null,
        "description": null,
        "plaid_account_id": null,
        "asset_id": null,
        "source": "manual",
        "amount": "-200.0000",
        "notes": null,
        "category_id": 911979,
        "category_group_id": null,
        "is_income": true,
        "exclude_from_totals": false,
        "granularity": "weeks",
        "quantity": 1,
        "occurrences": {
            "2024-05-29": [
                {
                    "id": 2178542342,
                    "date": "2024-05-29",
                    "amount": "-200",
                    "currency": "usd",
                    "payee": "Weekly Income",
                    "category_id": 911979,
                    "recurring_id": 940537,
                    "to_base": -200
                }
            ],
            "2024-06-05": [
                {
                    "id": 2178538493,
                    "date": "2024-06-05",
                    "amount": "-200",
                    "currency": "usd",
                    "payee": "Weekly Income",
                    "category_id": 911979,
                    "recurring_id": 940537,
                    "to_base": -200
                }
            ],
            "2024-06-12": [],
            "2024-06-19": [],
            "2024-06-26": [],
            "2024-07-03": []
        },
        "transactions_within_range": [
                {
                    "id": 2178538493,
                    "date": "2024-06-05",
                    "amount": "-200",
                    "currency": "usd",
                    "payee": "Weekly Income",
                    "category_id": 911979,
                    "recurring_id": 940537,
                    "to_base": -200
                }
        ],
        "missing_dates_within_range": [
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
        "payee": "Google Fi",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.698Z",
        "updated_at": "2024-04-05T20:02:22.698Z",
        "billing_date": "2024-01-25",
        "original_name": null,
        "description": "Cell phone plan",
        "plaid_account_id": null,
        "asset_id": 109419,
        "source": "manual",
        "amount": "50.0000",
        "notes": null,
        "category_id": 911972,
        "category_group_id": null,
        "is_income": false,
        "exclude_from_totals": false,
        "granularity": "months",
        "quantity": 1,
        "occurrences": {
            "2024-05-25": [
                {
                    "id": 21781233,
                    "date": "2024-05-25",
                    "amount": "50.0000",
                    "currency": "usd",
                    "payee": "Google Fi",
                    "category_id": 911972,
                    "recurring_id": 838965,
                    "to_base": 50
                }
            ],
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
        "id": 838963,
        "start_date": null,
        "end_date": null,
        "payee": "Geico",
        "currency": "usd",
        "created_at": "2024-04-05T20:02:22.244Z",
        "updated_at": "2024-04-05T20:02:22.244Z",
        "billing_date": "2024-01-01",
        "original_name": null,
        "description": "Car insurance",
        "plaid_account_id": null,
        "asset_id": 109419,
        "source": "manual",
        "amount": "145.0000",
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
    }
]```

> Example 404 Response

```json
{ "error": "Invalid start_date. Must be in format YYYY-MM-DD" }
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/recurring_items`

| Parameter         | Type    | Required | Default                        | Description                                                                                                                                                                   |
| ----------------- | ------- | -------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| start_date        | string  | false    | First day of the current month | Accepts a string in ISO-8601 short format.  If not set the recurring items for the current month are returned.  If set denotes the starting month of the date range to return |
| end_date          | string  | false    | none                           | Required if start_date is set.  Must be a date later than start_date.  Denotes the end month of the date range to return                                                      |
| debit_as_negative | boolean | false    | false                          | Pass in true if you’d like items to be returned as negative amounts and credits as positive amounts.                                                                          |

---
