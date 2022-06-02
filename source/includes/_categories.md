# Categories

## Categories Object

| Attribute Name      | Type             | Description                                                                                                                        |
| ------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| id                  | number           | A unique identifier for the category.                                                                                              |
| name                | string           | The name of the category. Must be between 1 and 40 characters.                                                                     |
| description         | string           | The description of the category. Must not exceed 140 characters.                                                                   |
| is_income           | boolean          | If true, the transactions in this category will be treated as income.                                                              |
| exclude_from_budget | boolean          | If true, the transactions in this category will be excluded from the budget.                                                       |
| exclude_from_totals | boolean          | If true, the transactions in this category will be excluded from totals.                                                           |
| updated_at          | string           | The date and time of when the category was last updated (in the ISO 8601 extended format).                                         |
| created_at          | string           | The date and time of when the category was created (in the ISO 8601 extended format).                                              |
| is_group            | boolean          | If true, the category is a group that can be a parent to other categories.                                                         |
| group_id            | number           | The ID of a category group (or null if the category doesn't belong to a category group).                                           |
| children            | array of objects | For category groups, this will populate with the categories nested within and include id, name, description and created_at fields. |

## Get All Categories

Use this endpoint to get a flattened list of all categories in alphabetical order associated with the user's account.

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

## Get Single Category

Use this endpoint to get hydrated details on a single category. Note that if this category is part of a category group, its properties (is_income, exclude_from_budget, exclude_from_totals) will inherit from the category group.

```json
{
  "id": 315358,
  "name": "Food & Drink",
  "description": null,
  "is_income": false,
  "exclude_from_budget": false,
  "exclude_from_totals": false,
  "is_group": true,
  "group_id": null,
  "children": [
    {
      "id": 315162,
      "name": "Alcohol, Bars",
      "description": null,
      "created_at": "2022-03-06T20:11:36.066Z"
    },
    {
      "id": 315169,
      "name": "Groceries",
      "description": null,
      "created_at": "2022-03-06T20:11:36.120Z"
    },
    {
      "id": 315172,
      "name": "Restaurants",
      "description": null,
      "created_at": "2022-03-06T20:11:36.146Z"
    }
  ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/categories/:category_id`

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
{
  "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance."
}
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/categories`

| Parameter           | Type    | Required | Default | Description                                                                             |
| ------------------- | ------- | -------- | ------- | --------------------------------------------------------------------------------------- |
| name                | string  | true     | -       | Name of category. Must be between 1 and 40 characters.                                  |
| description         | string  | false    | -       | Description of category. Must be less than 140 categories.                              |
| is_income           | boolean | false    | false   | Whether or not transactions in this category should be treated as income.               |
| exclude_from_budget | boolean | false    | false   | Whether or not transactions in this category should be excluded from budgets.           |
| exclude_from_totals | boolean | false    | false   | Whether or not transactions in this category should be excluded from calculated totals. |

## Create Category Group

Use this endpoint to create a single category group

> Example 200 Response
>
> Returns the ID of the newly-created category group

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
{
  "error": "A category with the same name (sample_duplicate_name) already exists."
}
```

```json
{
  "error": "The following category id(s) could not be added as a group because you do not have permissions for this category, or it is already a category group: 3893"
}
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/categories/group`

| Parameter           | Type             | Required | Default | Description                                                                                            |
| ------------------- | ---------------- | -------- | ------- | ------------------------------------------------------------------------------------------------------ |
| name                | string           | true     | -       | Name of category. Must be between 1 and 40 characters.                                                 |
| description         | string           | false    | -       | Description of category. Must be less than 140 categories.                                             |
| is_income           | boolean          | false    | false   | Whether or not transactions in this category should be treated as income.                              |
| exclude_from_budget | boolean          | false    | false   | Whether or not transactions in this category should be excluded from budgets.                          |
| exclude_from_totals | boolean          | false    | false   | Whether or not transactions in this category should be excluded from calculated totals.                |
| category_ids        | array of numbers | false    | -       | Array of category_id to include in the category group.                                                 |
| new_categories      | array of strings | false    | -       | Array of strings representing new categories to create and subsequently include in the category group. |

## Update Category

Use this endpoint to update the properties for a single category or category group

> Example 200 Response
>
> If successfully updated, returns true

```json
true
```

> Example Error Response (sends as 200)

```json
{ "error": "No valid fields to update for this category." }
```

```json
{ "error": "You may not set the is_group property for an existing category." }
```

```json
{
  "error": "This category cannot be assigned a group because it is a category group."
}
```

```json
{
  "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance."
}
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/categories/:category_id`

| Parameter           | Type    | Required | Default | Description                                                                             |
| ------------------- | ------- | -------- | ------- | --------------------------------------------------------------------------------------- |
| name                | string  | true     | -       | Name of category. Must be between 1 and 40 characters.                                  |
| description         | string  | false    | -       | Description of category. Must be less than 140 categories.                              |
| is_income           | boolean | false    | false   | Whether or not transactions in this category should be treated as income.               |
| exclude_from_budget | boolean | false    | false   | Whether or not transactions in this category should be excluded from budgets.           |
| exclude_from_totals | boolean | false    | false   | Whether or not transactions in this category should be excluded from calculated totals. |
| group_id            | number  | false    | false   | For a category, set the group_id to include it in a category group                      |

## Add to Category Group

Use this endpoint to add categories (either existing or new) to a single category group

> Example 200 Response
>
> Returns the hydrated object for the category group

```json
{
  "id": 315358,
  "name": "Food & Drink",
  "description": null,
  "is_income": false,
  "exclude_from_budget": false,
  "exclude_from_totals": false,
  "is_group": true,
  "group_id": null,
  "children": [
    {
      "id": 315162,
      "name": "Alcohol, Bars",
      "description": null,
      "created_at": "2022-03-06T20:11:36.066Z"
    },
    {
      "id": 315164,
      "name": "Coffee Shops",
      "description": null,
      "created_at": "2022-03-06T20:11:36.082Z"
    },
    {
      "id": 315169,
      "name": "Groceries",
      "description": null,
      "created_at": "2022-03-06T20:11:36.120Z"
    },
    {
      "id": 315172,
      "name": "Restaurants",
      "description": null,
      "created_at": "2022-03-06T20:11:36.146Z"
    }
  ]
}
```

> Example Error Response (sends as 200)

```json
{
  "error": "The following category id(s) could not be added as a group because you do not have permissions for this category, or it is already a category group: 3280"
}
```

```json
{
  "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance."
}
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/categories/group/:group_id/add`

| Parameter      | Type             | Required | Default | Description                                                                                            |
| -------------- | ---------------- | -------- | ------- | ------------------------------------------------------------------------------------------------------ |
| category_ids   | array of numbers | false    | -       | Array of category_id to include in the category group.                                                 |
| new_categories | array of strings | false    | -       | Array of strings representing new categories to create and subsequently include in the category group. |

## Delete Category

Use this endpoint to delete a single category or category group. This will only work if there are no dependencies, such as existing budgets for the category, categorized transactions, categorized recurring items, etc. If there are dependents, this endpoint will return what the dependents are and how many there are.

> Example 200 Response
>
> Returns the ID of the newly-created category group

```json
{
  "success": true
}
```

> Example Error Response (sends as 200)

```json
{
  "dependents": {
    "category_name": "Food & Drink",
    "budget": 4,
    "category_rules": 0,
    "transactions": 43,
    "children": 7,
    "recurring": 0
  }
}
```

### HTTP Request

`DELETE https://dev.lunchmoney.app/v1/categories/:category_id`

## Force Delete Category

Use this endpoint to force delete a single category or category group and along with it, disassociate the category from any transactions, recurring items, budgets, etc. Note: it is best practice to first try the `Delete Category` endpoint to ensure you don't accidentally delete any data. Disassociation/deletion of the data arising from this endpoint is irreversible!

> Example 200 Response
>
> Returns the ID of the newly-created category group

```json
{
  "success": true
}
```

> Example Error Response (sends as 200)

```json
{
  "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance."
}
```

### HTTP Request

`DELETE https://dev.lunchmoney.app/v1/categories/:category_id/force`

---
