# Budget

## Budget Object

Attribute Name      | Type   | Description
------------------- | ----   | -----------
category_name       | string | Name of the category
category_id         | number | Unique identifier for category
category_group_name | string | Name of the category group, if applicable
group_id            | number | Unique identifier for category group
is_group            | boolean | If true, this category is a group
is_income           | boolean | If true, this category is an income category (category properties are set in the app via the Categories page)
exclude_from_budget | boolean | If true, this category is excluded from budget (category properties are set in the app via the Categories page)
exclude_from_totals | boolean | If true, this category is excluded from totals (category properties are set in the app via the Categories page)
data                | Array of data objects | For each month with budget or category spending data, there is a data object with the key set to the month in format YYYY-MM-DD. For properties, see Data object below.


## Data Object

Attribute Name      | Type   | Description
------------------- | ----   | -----------
budget_month        | number | Month of the budget in YYYY-MM-DD format
budget_amount       | number | The budget amount, as set by the user
budget_currency     | string | The budget currency, as set by the user
budget_to_base      | number | The budget converted to the user's primary currency. If the multicurrency feature is not being used, budget_to_base and budget_amount will be the same.
spending_to_base    | number | The total amount spent in this category for this time period in the user's primary currency
num_transactions    | number | Number of transactions that make up "spending_to_base"

## Get Budget Summary
Use this endpoint to get full details on the budgets for all categories between a certain time period. The budgeted and spending amounts will be an aggregate across this time period.

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
                "spending_to_base": 373.51
            },
            "2020-08-01": {
                "spending_to_base": 1387.14
            },
            "2020-07-01": {
                "spending_to_base": 1720.46
            },
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
                "budget_month": "2020-09-01",
                "budget_to_base": null,
                "budget_amount": null,
                "budget_currency": null,
                "spending_to_base": 270.86,
                "num_transactions": 14
            },
            "2020-08-01": {
                "budget_month": "2020-08-01",
                "budget_to_base": null,
                "budget_amount": null,
                "budget_currency": null,
                "spending_to_base": 79.53,
                "num_transactions": 8
            },
            "2020-07-01": {
                "budget_month": "2020-07-01",
                "budget_to_base": null,
                "budget_amount": null,
                "budget_currency": null,
                "spending_to_base": 149.61,
                "num_transactions": 8
            }
        },
        "order": 1
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/budgets`

## Upsert budget

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
amount           | number | false    | -       | Amount for budget
currency         | string | false    | -       | Currency for the budgeted amount (optional). If empty, will default to your primary currency
start_date       | string | false    | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01)
category_id      | number | false    | -       | Unique identifier for the category

## Remove budget

Use this endpoint to unset an existing budget for a particular category in a particular month.

> Example 200 Response

```json
true
```

> Example Error Response (sends as 200)

```json
{ "error": 'start_date must be a valid date in format YYYY-MM-01' }
```

### HTTP Request

`DELETE https://dev.lunchmoney.app/v1/budgets`

### Query Parameters
Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
start_date       | string | false    | -       | Start date for the budget period. Lunch Money currently only supports monthly budgets, so your date must always be the start of a month (eg. 2021-04-01)
category_id      | number | false    | -       | Unique identifier for the category

---
