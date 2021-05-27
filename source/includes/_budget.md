# Budget

## Budget Object
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
            "2020-03-01": {
                "budget_month": "2020-03-01",
                "budget_to_base": 1110,
                "budget_amount": 1110,
                "budget_currency": "usd",
                "spending_to_base": 873.4799999999999,
                "num_transactions": 0
            },
            "2020-01-01": {
                "spending_to_base": 1199.9
            },
            "2020-02-01": {
                "spending_to_base": 1190.56
            },
            "2020-04-01": {
                "spending_to_base": 1165.7700000000002
            },
            "2020-05-01": {
                "spending_to_base": 1236.4400000000003
            },
            "2020-06-01": {
                "spending_to_base": 1336.3900000000003
            },
            "2020-12-01": {
                "spending_to_base": 1364.7800000000002
            },
            "2020-11-01": {
                "spending_to_base": 1481.0600000000002
            },
            "2020-10-01": {
                "spending_to_base": 1639.94
            },
            "2020-09-01": {
                "spending_to_base": 373.51000000000016
            },
            "2020-08-01": {
                "spending_to_base": 1387.14
            },
            "2020-07-01": {
                "spending_to_base": 1720.4699999999996
            },
            "2021-01-01": {
                "spending_to_base": 23.66
            }
        },
        "order": 0
    },


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
                "spending_to_base": 373.51000000000016
            },
            "2020-08-01": {
                "spending_to_base": 1387.14
            },
            "2020-07-01": {
                "spending_to_base": 1720.4699999999996
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

`GET https://dev.lunchmoney.app/v1/crypto`

## Update Manual Crypto Asset

Use this endpoint to update a single manually-managed crypto asset (does not include assets received from syncing with your wallet/exchange/etc). These are denoted by `source: manual` from the GET call above.

> Example 200 Response

```json
{
    "id": 1,
    "source": "manual",
    "created_at": "2021-02-10T05:57:34.305Z",
    "name": "Shiba Token",
    "display_name": "SHIB",
    "balance": "12.000000000000000000",
    "currency": "SHIB",
    "status": "active",
    "institution_name": null
}
```

> Example 400 Response

```json
{ "errors": [ "currency is invalid for crypto: ${requestBody.currency}. It may not be supported yet. Request to get it supported via the app or support@lunchmoney.app" ] }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/crypto/manual/:id`

### Body Parameters
Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
name             | string | false    | -       | Official or full name of the account. Max 45 characters
display_name     | string | false    | -       | Display name for the account. Max 25 characters
institution_name | string | false    | -       | Name of provider that holds the account. Max 50 characters
balance          | number | false    | -       | Numeric value of the current balance of the account. Do not include any special characters aside from a decimal point!
currency         | string | false    | -       | Cryptocurrency that is supported for manual tracking in our database

---
