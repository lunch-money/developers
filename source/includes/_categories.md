# Categories

## Categories Object
Attribute Name      | Type    | Description
------------------- | ----    | -----------
id                  | number  | A unique identifier for the category.
name                | string  | The name of the category. Must be between 1 and 40 characters.
description         | string  | The description of the category. Must not exceed 140 characters.
is_income           | boolean | If true, the transactions in this category will be treated as income.
exclude_from_budget | boolean | If true, the transactions in this category will be excluded from the budget.
exclude_from_totals | boolean | If true, the transactions in this category will be excluded from totals.
updated_at          | string  | The date and time of when the category was last updated (in the ISO 8601 extended format).
created_at          | string  | The date and time of when the category was created (in the ISO 8601 extended format).
is_group            | boolean | If true, the category is a group that can be a parent to other categories.
group_id            | number  | The ID of a category group (or null if the category doesn't belong to a category group).

## Get All Categories
Use this endpoint to get a list of all categories associated with the user's account.

```json
{
  "categories": [
    {
      "id": 83,
      "name": "Test 1",
      "description": "Test description",
      "is_income": false,
      "exclude_from_budget": true,
      "exclude_from_totals": false,
      "updated_at": "2020-01-28T09:49:03.225Z",
      "created_at": "2020-01-28T09:49:03.225Z",
      "is_group": true,
      "group_id": null
    },
    {
      "id": 84,
      "name": "Test 2",
      "description": null,
      "is_income": true,
      "exclude_from_budget": false,
      "exclude_from_totals": true,
      "updated_at": "2020-01-28T09:49:03.238Z",
      "created_at": "2020-01-28T09:49:03.238Z",
      "is_group": false,
      "group_id": 83
    }
  ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/categories`

## Create Category
Use this endpoint to create a single category

> Example 200 Response
>
> Returns the ID of the newly-created category


```json
{
    "category_id": 26213
}
```

> Example Error Response (sends as 200)

```json
{ "error": "Missing category name." }
```
```json
{ "error": "Category name must be less than 40 characters." }
```
```json
{ "error": "Category description must be less than 140 characters." }
```
```json
{ "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance." }
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/categories`

Parameter           | Type    | Required   | Default   | Description
----------          | ------  | ---------- | --------- | ------------
name                | string  | true       | -         | Name of category. Must be between 1 and 40 characters.
description         | string  | false      | -         | Description of category. Must be less than 140 categories.
is_income           | boolean | false      | false     | Whether or not transactions in this category should be treated as income.
exclude_from_budget | boolean | false      | false     | Whether or not transactions in this category should be excluded from budgets.
exclude_from_totals | boolean | false      | false     | Whether or not transactions in this category should be excluded from calculated totals.

---
