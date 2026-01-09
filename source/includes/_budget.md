# Budget

The v1/budgets API is deprecated.  Developers are encouraged to migrate their application to the new [v2/summary API](https://alpha.lunchmoney.dev/v2/docs#tag/summary) instead.

## Budget Object

| Attribute Name      | Type    | Nullable | Description                                                                                                                                                              |
| ------------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| category_name       | string  | false    | Name of the category, will be "Uncategorized" if no category is assigned                                                                                                 |
| category_id         | number  | true     | Unique identifier for category, can be null when `category_name` is "Uncategorized"                                                                                      |
| category_group_name | string  | true     | Name of the category group, if applicable                                                                                                                                |
| group_id            | number  | true     | Unique identifier for category group                                                                                                                                     |
| is_group            | boolean | true     | If true, this category is a group                                                                                                                                        |
| is_income           | boolean | false    | If true, this category is an income category (category properties are set in the app via the Categories page)                                                            |
| exclude_from_budget | boolean | false    | If true, this category is excluded from budget (category properties are set in the app via the Categories page)                                                          |
| exclude_from_totals | boolean | false    | If true, this category is excluded from totals (category properties are set in the app via the Categories page)                                                          |
| data                | Data[]  | false    | For each month with budget or category spending data, there is a data object with the key set to the month in format YYYY-MM-DD. For properties, see Data object below.  |
| config              | object  | true     | Object representing the category's budget suggestion configuration. Will be `null` if the category has no budget configuration set. See Config Object below for details. |
| order               | number  | false    | Numerical ordering of budgets                                                                                                                                            |
| archived            | boolean | false    | True if the category is archived and not displayed in relevant areas of the Lunch Money app.                                                                             |
| recurring           | object  | true     | Returns a list object that contains an array of Recurring Expenses objects (just the `payee`, `amount`, `currency`, and `to_base` fields) that affect this budget        |

**Note:** A category may appear in the budget response even if no budget has been set for it. This can happen when:
- The category has transactions but no budget entries have been created
- The category has a custom budget period configured but no budget amounts have been set
- Budget entries were deleted but transactions still exist for that category
- After a users existing v1 budgets were migrated to v2 budgets, if a user changed their budget period settings (quantity, granularity, or anchor date), categories without budget values may exist.

In these cases, the `config` property will be `null`, and the `data` object will contain spending information (`spending_to_base`, `num_transactions`) but all `budget_*` properties will be `null`.

## Data Object

| Attribute Name   | Type    | Nullable | Description                                                                                                                                                                                                                         |
| ---------------- | ------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| budget_amount    | number  | true     | The budget amount, as set by the user. Will be `null` if no budget has been set for this category and time period.                                                                                                                  |
| budget_currency  | string  | true     | The budget currency, as set by the user. Will be `null` if no budget has been set for this category and time period.                                                                                                                |
| budget_to_base   | number  | true     | The budget converted to the user's primary currency. If the multicurrency feature is not being used, budget_to_base and budget_amount will be the same. Will be `null` if no budget has been set for this category and time period. |
| spending_to_base | number  | false    | The total amount spent in this category for this time period in the user's primary currency. This value will always be present, even if no budget has been set.                                                                     |
| num_transactions | number  | false    | Number of transactions that make up "spending_to_base". This value will always be present, even if no budget has been set.                                                                                                          |
| is_automated     | boolean | true     | If true, the budget_amount is only a suggestion based on the set config. If not present, it is false (meaning this is a locked in budget). Will be `null` if no budget has been set.                                                |

**Note:** When a category has transactions but no budget entries, the `data` object will still be present with spending information, but all `budget_*` properties will be `null`. This allows you to see spending activity even when budgets haven't been configured.

## Config Object

| Attribute Name | Type   | Nullable | Description                                                                                                                                                              |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| config_id      | number | false    | Unique identifier for config                                                                                                                                             |
| cadence        | string | false    | One of:<br> <ul> <li>monthly</li> <li>twice a month</li> <li>once a week</li> <li>every 3 months</li> <li>every 4 months</li> <li>twice a year</li> <li>yearly</li></ul> |
| amount         | number | false    | Amount in numeric format                                                                                                                                                 |
| currency       | string | false    | Three-letter lowercase currency code for the recurring expense in ISO 4217 format                                                                                        |
| to_base        | number | false    | The amount converted to the user's primary currency.                                                                                                                     |
| auto_suggest   | string | false    | One of:<br> <ul> <li>budgeted</li> <li>fixed</li> <li>fixed-rollover</li> <li>spent</li> </ul>                                                                           |

**Note:** The `config` property in the Budget Object can be `null`. When `config` is `null`, it means the category has no budget configuration set. When `config` is present (not null), all properties listed above are required and not nullable.

## Get Budget Summary

Use this endpoint to get full details on the budgets for all budgetable categories between a certain time period. The budgeted and spending amounts will be broken down by month.

**Note:** Categories may appear in the response even if they have no budget entries, as long as they have transactions in the specified time period. In these cases, `config` will be `null` and all `budget_*` properties in the `data` object will be `null`, but `spending_to_base` and `num_transactions` will still be populated.

> Example 200 Response

```json
[
    {
        "category_name": "Food",
        "category_id": 34476,
        "category_group_name": null,
        "group_id": null,
        "is_group": true,
        "is_income": false,
        "exclude_from_budget": false,
        "exclude_from_totals": false,
        "data": {
            "2020-09-01": {
                "num_transactions": 23,
                "spending_to_base": 373.51,
                "budget_to_base": 376.08,
                "budget_amount": 376.08,
                "budget_currency": "usd",
                "is_automated": true,
            },
            "2020-08-01": {
                "num_transactions": 23,
                "spending_to_base": 123.92,
                "budget_to_base": 300,
                "budget_amount": 300,
                "budget_currency": "usd",
            },
            "2020-07-01": {
                "num_transactions": 23,
                "spending_to_base": 228.66,
                "budget_to_base": 300,
                "budget_amount": 300,
                "budget_currency": "usd",
            }
        },
        "config": {
            "config_id": 9,
            "cadence": "monthly",
            "amount": 300,
            "currency": "usd",
            "to_base": 300,
            "auto_suggest": "fixed-rollover"
        },
        "order": 0,
        "archived": false,
        "recurring": null
    },
    {
        "category_name": "Alcohol & Bars",
        "category_id": 26,
        "category_group_name": "Food",
        "group_id": 34476,
        "is_group": null,
        "is_income": false,
        "exclude_from_budget": false,
        "exclude_from_totals": false,
        "data": {
            "2020-09-01": {
                "spending_to_base": 270.86,
                "num_transactions": 14
            },
            "2020-08-01": {
                "spending_to_base": 79.53,
                "num_transactions": 8
            },
            "2020-07-01": {
                "spending_to_base": 149.61,
                "num_transactions": 8
            }
        },
        "config": null,
        "archived": false,
        "order": 1,
        "recurring": {
            "list": [
                {
                    "payee": "Recurring Payee",
                    "amount": "20.000",
                    "currency": "cad",
                    "to_base": 20
                }
            ]
        }
    },
    {
        "category_name": "Unbudgeted Category",
        "category_id": 12345,
        "category_group_name": null,
        "group_id": null,
        "is_group": false,
        "is_income": false,
        "exclude_from_budget": false,
        "exclude_from_totals": false,
        "data": {
            "2020-09-01": {
                "spending_to_base": 50.00,
                "num_transactions": 1,
                "budget_to_base": null,
                "budget_amount": null,
                "budget_currency": null
            }
        },
        "config": null,
        "order": 2,
        "archived": false,
        "recurring": null
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/budgets`

### Query Parameters

| Parameter  | Type   | Required | Default | Description                                                                                                                                         |
| ---------- | ------ | -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| start_date | string | true     | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date should be the start of a month (eg. 2021-04-01) |
| end_date   | string | true     | -       | End date for the budget period. Lunch Money currently only supports monthly budgets, so your date should be the end of a month (eg. 2021-04-30)     |
| currency   | string | false    | -       | Currency for the budgeted amount (optional). If empty, will default to your primary currency                                                        |

## Upsert Budget

Use this endpoint to update an existing budget or insert a new budget for a particular category and date.

Note: Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01)

> Example 200 Response

If this is a sub-category, the response will include the updated category group's budget. This is because setting a sub-category may also update the category group's overall budget.

```json
{
    "category_group": {
        "category_id": 34476,
        "amount": 100,
        "currency": "usd",
        "start_date": "2021-06-01"
    }
}
```

> Example Error Response (sends as 200)

```json
{ "error": "Budget must be greater than or equal to the sum of sub-category budgets ($10.01)." }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/budgets`

### Body Parameters

| Parameter   | Type   | Required | Default | Description                                                                                                                                              |
| ----------- | ------ | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| start_date  | string | true     | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01) |
| category_id | number | true     | -       | Unique identifier for the category                                                                                                                       |
| amount      | number | true     | -       | Amount for budget                                                                                                                                        |
| currency    | string | false    | -       | Currency for the budgeted amount (optional). If empty, will default to your primary currency                                                             |

## Remove Budget

Use this endpoint to unset an existing budget for a particular category in a particular month.

> Example 200 Response

```json
true
```

> Example Error Response (sends as 200)

```json
{ "error": "start_date must be a valid date in format YYYY-MM-01" }
```

### HTTP Request

`DELETE https://dev.lunchmoney.app/v1/budgets`

### Query Parameters

| Parameter   | Type   | Required | Default | Description                                                                                                                                              |
| ----------- | ------ | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| start_date  | string | true     | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01) |
| category_id | number | true     | -       | Unique identifier for the category                                                                                                                       |

---
