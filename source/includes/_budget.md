# Budget

## Budget Object

Attribute Name      | Type   | Nullable | Description
------------------- | ----   | -------- | -----------
category_name       | string | false | Name of the category, will be "Uncategorized" if no category is assigned
category_id         | number | true | Unique identifier for category, can be null when `category_name` is "Uncategorized"
category_group_name | string | true | Name of the category group, if applicable
group_id            | number | true | Unique identifier for category group
is_group            | boolean | true | If true, this category is a group
is_income           | boolean | false | If true, this category is an income category (category properties are set in the app via the Categories page)
exclude_from_budget | boolean | false | If true, this category is excluded from budget (category properties are set in the app via the Categories page)
exclude_from_totals | boolean | false | If true, this category is excluded from totals (category properties are set in the app via the Categories page)
data                | Array of data objects | false | For each month with budget or category spending data, there is a data object with the key set to the month in format YYYY-MM-DD. For properties, see Data object below.
config              | object | true | Object representing the category's budget suggestion configuration

## Data Object

Attribute Name      | Type   | Nullable | Description
------------------- | ----   | -------- | -----------
budget_amount       | number | true | The budget amount, as set by the user. If empty, no budget has been set.
budget_currency     | string | true | The budget currency, as set by the user. If empty, no budget has been set.
budget_to_base      | number | true | The budget converted to the user's primary currency. If the multicurrency feature is not being used, budget_to_base and budget_amount will be the same. If empty, no budget has been set.
spending_to_base    | number | false | The total amount spent in this category for this time period in the user's primary currency
num_transactions    | number | false | Number of transactions that make up "spending_to_base"
is_automated        | boolean | true | If true, the budget_amount is only a suggestion based on the set config. If not present, it is false (meaning this is a locked in budget)

## Config Object

Attribute Name      | Type   | Nullable | Description
------------------- | ----   | -------- | -----------
config_id           | number | false | Unique identifier for config
cadence             | string | false | One of:<br> <ul> <li>monthly</li> <li>twice a month</li> <li>once a week</li> <li>every 3 months</li> <li>every 4 months</li> <li>twice a year</li> <li>yearly</li></ul>
amount              | number | false | Amount in numeric format
currency            | string | false | Three-letter lowercase currency code for the recurring expense in ISO 4217 format
to_base             | number | false | The amount converted to the user's primary currency.
auto_suggest        | string | false | One of:<br> <ul> <li>budgeted</li> <li>fixed</li> <li>fixed-rollover</li> <li>spent</li> </ul>

## Get Budget Summary

Use this endpoint to get full details on the budgets for all budgetable categories between a certain time period. The budgeted and spending amounts will be broken down by month.

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
        "order": 0
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
        "order": 1
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/budgets`

### Query Parameters

Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
start_date       | string | true    | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date should be the start of a month (eg. 2021-04-01)
end_date         | string | true    | -       | End date for the budget period. Lunch Money currently only supports monthly budgets, so your date should be the end of a month (eg. 2021-04-30)
currency         | string | false    | -       | Currency for the budgeted amount (optional). If empty, will default to your primary currency

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

Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
start_date       | string | true    | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01)
category_id      | number | true    | -       | Unique identifier for the category
amount           | number | true    | -       | Amount for budget
currency         | string | false    | -       | Currency for the budgeted amount (optional). If empty, will default to your primary currency

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

Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
start_date       | string | true    | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01)
category_id      | number | true    | -       | Unique identifier for the category

---
