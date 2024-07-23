# Transactions

## Transaction Object

| Attribute Name             | Type          | Nullable | Description                                                                                                                                                                                                                                                                                                     |
| -------------------------- | ------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                         | number        | false    | Unique identifier for transaction                                                                                                                                                                                                                                                                               |
| date                       | string        | false    | Date of transaction in ISO 8601 format                                                                                                                                                                                                                                                                          |
| payee                      | string        | false    | Name of payee. If recurring_id is not null, this field will show the payee of associated recurring expense instead of the original transaction payee                                                                                                                                                            |
| amount                     | string        | false    | Amount of the transaction in numeric format to 4 decimal places                                                                                                                                                                                                                                                 |
| currency                   | string        | false    | Three-letter lowercase currency code of the transaction in ISO 4217 format                                                                                                                                                                                                                                      |
| to_base                    | number        | false    | The amount converted to the user's primary currency. If the multicurrency feature is not being used, to_base and amount will be the same.                                                                                                                                                                       |
| category_id                | number        | true     | Unique identifier of associated category (see Categories)                                                                                                                                                                                                                                                       |
| category_name              | string        | true     | Name of category associated with transaction                                                                                                                                                                                                                                                                    |
| category_group_id          | number        | true     | Unique identifier of associated category group, if any                                                                                                                                                                                                                                                          |
| category_group_name        | string        | true     | Name of category group associated with transaction, if any                                                                                                                                                                                                                                                      |
| is_income                  | boolean       | false    | Based on the associated category's property, denotes if transaction is treated as income                                                                                                                                                                                                                        |
| exclude_from_budget        | boolean       | false    | Based on the associated category's property, denotes if transaction is excluded from budget                                                                                                                                                                                                                     |
| exclude_from_totals        | boolean       | false    | Based on the associated category's property, denotes if transaction is excluded from totals                                                                                                                                                                                                                     |
| created_at                 | string        | false    | The date and time of when the transaction was created (in the ISO 8601 extended format).                                                                                                                                                                                                                        |
| updated_at                 | string        | false    | The date and time of when the transaction was last updated (in the ISO 8601 extended format).                                                                                                                                                                                                                   |
| status                     | string        | true     | One of the following: <ul> <li>cleared: User has reviewed the transaction</li><li>uncleared: User has not yet reviewed the transaction</li><li>pending: Imported transaction is marked as pending. This should be a temporary state.</li></ul> Note: special statuses for recurring items have been deprecated. |
| is_pending                 | boolean       | false    | Denotes if transaction is pending (not posted)                                                                                                                                                                                                                                                                  |
| notes                      | string        | true     | User-entered transaction notes If recurring_id is not null, this field will be description of associated recurring expense                                                                                                                                                                                      |
| original_name              | string        | true     | The transactions original name before any payee name updates. For synced transactions, this is the raw original payee name from your bank.                                                                                                                                                                      |
| recurring_id               | number        | true     | Unique identifier of associated recurring item, or null for non-recurring transactions                                                                                                                                                                       |
| recurring_payee            | string        | true    | The payee of associated recurring expense instead of the original transaction, or null for non-recurring transactions                                                                                                                                                                    |
| recurring_description      | string        | true     | Description of associated recurring item, or null for non-recurring transactions                                                                                                                                                                                                                                                                        |
| recurring_cadence          | string        | true     | Cadence of associated recurring item (one of `once a week`, `every 2 weeks`, `twice a month`, `monthly`, `every 2 months`, `every 3 months`, `every 4 months`, `twice a year`, `yearly`)  , or null for non-recurring transactions                                                                                                                     |
| recurring_type             | string        | true     | Type of associated recurring (one of `cleared`, `suggested`, `dismissed`), or null for non-recurring transactions                                                                                                                                                                                                                                       |
| recurring_amount           | number        | true     | Amount of associated recurring item, or null for non-recurring transactions                                                                                                                                                                                                                                                                            |
| recurring_currency         | number        | true     | Currency of associated recurring item, or null for non-recurring transactions                                                                                                                                                                                                                                                                          |
| parent_id                  | number        | true     | Exists if this is a split transaction. Denotes the transaction ID of the original transaction.                                                                                                                                                   |
| has_children               | boolean       | false    | True if this transaction is a parent transaction and is split into 2 or more other transactions                                                                                                                                                                                                                 |
| group_id                   | number        | true     | Exists if this transaction is part of a group. Denotes the parent’s transaction ID                                                                                                                                                                                                                              |
| is_group                   | boolean       | false    | True if this transaction represents a group of transactions. If so, amount and currency represent the totalled amount of transactions bearing this transaction’s id as their group_id. Amount is calculated based on the user’s primary currency.                                                               |
| asset_id                   | number        | true     | Unique identifier of associated manually-managed account (see Assets) Note: plaid_account_id and asset_id cannot both exist for a transaction                                                                                                                                                                   |
| asset_institution_name     | number        | true     | Institution name of associated manually-managed account                                                                                                                                                                                                                                                         |
| asset_name                 | number        | true     | Name of associated manually-managed account                                                                                                                                                                                                                                                                     |
| asset_display_name         | number        | true     | Display name of associated manually-managed account                                                                                                                                                                                                                                                             |
| asset_status               | number        | true     | Status of associated manually-managed account (one of `active`, `closed`)                                                                                                                                                                                                                                       |
| plaid_account_id           | number        | true     | Unique identifier of associated Plaid account (see Plaid Accounts) Note: plaid_account_id and asset_id cannot both exist for a transaction                                                                                                                                                                      |
| plaid_account_name         | number        | true     | Name of associated Plaid account                                                                                                                                                                                                                                                                                |
| plaid_account_mask         | number        | true     | Mask of associated Plaid account                                                                                                                                                                                                                                                                                |
| institution_name           | number        | true     | Institution name of associated Plaid account                                                                                                                                                                                                                                                                    |
| plaid_account_display_name | number        | true     | Display name of associated Plaid account                                                                                                                                                                                                                                                                        |
| plaid_metadata             | number        | true     | Metadata associated with imported transaction from Plaid                                                                                                                                                                                                                                                        |
| source                     | number        | false    | Source of the transaction (one of `api`, `csv`, `manual`,`merge`,`plaid`,`recurring`,`rule`,`user`)                                                                                                                                                                                                             |
| display_name               | number        | false    | Display name for payee for transaction based on whether or not it is linked to a recurring item. If linked, returns `recurring_payee` field. Otherwise, returns the `payee` field.                                                                                                                              |
| display_notes              | number        | true     | Display notes for transaction based on whether or not it is linked to a recurring item. If linked, returns `recurring_notes` field. Otherwise, returns the `notes` field.                                                                                                                                       |
| account_display_name       | number        | false    | Display name for associated account (manual or Plaid). If this is a synced account, returns `plaid_account_display_name` or `asset_display_name`.                                                                                                                                                               |
| tags                       | Tag[]         | false    | Array of Tag objects                                                                                                                                                                                                                                                                                            |
| children                   | Transaction[] | true     | Array of child Transaction objections, these objects are slimmed down to the more essential fields, and contain an extra field called `formatted_date` that contains the date of transaction in ISO 8601 format                                                                                                 |
| external_id                | string        | true     | User-defined external ID for any manually-entered or imported transaction. External ID cannot be accessed or changed for Plaid-imported transactions. External ID must be unique by asset_id. Max 75 characters.                                                                                                |
| original_date              | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |
| type                       | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |
| subtype                    | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |
| fees                       | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |
| price                      | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |
| quantity                   | string        | true     | DEPRECATED                                                                                                                                                                                                                                                                                                      |

## Get All Transactions

Use this endpoint to retrieve all transactions between a date range.

> Example 200 Response

```json
{
  "transactions": [
    {
      "id": 246946944,
      "date": "2023-07-18",
      "amount": "53.1900",
      "currency": "usd",
      "to_base": 53.19,
      "payee": "Amazon",
      "category_id": 315172,
      "category_name": "Restaurants",
      "category_group_id": 315358,
      "category_group_name": "Food & Drink",
      "is_income": false,
      "exclude_from_budget": false,
      "exclude_from_totals": false,
      "created_at": "2023-09-09T08:43:05.875Z",
      "updated_at": "2023-10-09T06:07:03.105Z",
      "status": "cleared",
      "is_pending": false,
      "notes": null,
      "original_name": null,
      "recurring_id": null,
      "recurring_payee": null,
      "recurring_description": null,
      "recurring_cadence": null,
      "recurring_type": null,
      "recurring_amount": null,
      "recurring_currency": null,
      "parent_id": 225508713,
      "has_children": false,
      "group_id": null,
      "is_group": false,
      "asset_id": null,
      "asset_institution_name": null,
      "asset_name": null,
      "asset_display_name": null,
      "asset_status": null,
      "plaid_account_id": 76602,
      "plaid_account_name": "Amazon Whole Foods Visa",
      "plaid_account_mask": "6299",
      "institution_name": "Chase",
      "plaid_account_display_name": "Amazon Whole Foods Visa",
      "plaid_metadata": null,
      "plaid_category": null,
      "source": null,
      "display_name": "Amazon",
      "display_notes": null,
      "account_display_name": "Amazon Whole Foods Visa",
      "tags": [
        {
          "name": "Amazon",
          "id": 76543
        }
      ],
      "external_id": null
    },
    {
      "id": 246946943,
      "date": "2023-07-18",
      "amount": "12.2100",
      "currency": "usd",
      "to_base": 12.21,
      "payee": "Frelard Tamales",
      "category_id": 315172,
      "category_name": "Restaurants",
      "category_group_id": 315358,
      "category_group_name": "Food & Drink",
      "is_income": false,
      "exclude_from_budget": false,
      "exclude_from_totals": false,
      "created_at": "2023-09-09T08:43:05.818Z",
      "updated_at": "2023-10-09T06:07:03.529Z",
      "status": "cleared",
      "is_pending": false,
      "notes": null,
      "original_name": null,
      "recurring_id": null,
      "recurring_payee": null,
      "recurring_description": null,
      "recurring_cadence": null,
      "recurring_type": null,
      "recurring_amount": null,
      "recurring_currency": null,
      "parent_id": 225588844,
      "has_children": false,
      "group_id": null,
      "is_group": true,
      "asset_id": null,
      "asset_institution_name": null,
      "asset_name": null,
      "asset_display_name": null,
      "asset_status": null,
      "plaid_account_id": 54174,
      "plaid_account_name": "Amex -11005",
      "plaid_account_mask": "1005",
      "institution_name": "American Express",
      "plaid_account_display_name": "Amex Plat",
      "plaid_metadata": null,
      "plaid_category": null,
      "source": null,
      "display_name": "Frelard Tamales",
      "display_notes": null,
      "account_display_name": "Amex Plat",
      "tags": [],
      "children": [
        {
          "id": 246946948,
          "payee": "Child Transaction One",
          "amount": "-33.6000",
          "currency": "cad",
          "date": "2023-08-10",
          "formatted_date": "2023-09-10",
          "notes": null,
          "asset_id": 7409,
          "plaid_account_id": null,
          "to_base": -33.6
        },
        {
          "id": 246946948,
          "payee": "Child Transaction Two",
          "amount": "-33.6000",
          "currency": "cad",
          "date": "2023-08-10",
          "formatted_date": "2023-09-10",
          "notes": null,
          "asset_id": 7409,
          "plaid_account_id": null,
          "to_base": -33.6
        }
      ]
      "external_id": null
    }
  ],
  "has_more": true
}
```

> Example 404 Response

```json
{ "error": "Both start_date and end_date must be specified." }
```

Returns list of Transaction objects and a `has_more` indicator. If no query parameters are set, this endpoint will return transactions for the current calendar month (see `start_date` and `end_date`)

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions`

### Query Parameters

| Parameter         | Type    | Required | Default | Description                                                                                                                                                  |
| ----------------- | ------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tag_id            | number  | false    | -       | Filter by tag. Only accepts IDs, not names.                                                                                                                  |
| recurring_id      | number  | false    | -       | Filter by recurring expense                                                                                                                                  |
| plaid_account_id  | number  | false    | -       | Filter by Plaid account                                                                                                                                      |
| category_id       | number  | false    | -       | Filter by category. Will also match category groups.                                                                                                         |
| asset_id          | number  | false    | -       | Filter by asset                                                                                                                                              |
| is_group          | boolean | false    | -       | Filter by group (returns transaction groups)                                                                                                                 |
| status            | string  | false    | -       | Filter by status (Can be `cleared` or `uncleared`. Note: special statuses for recurring items have been deprecated.)                                         |
| start_date        | string  | false    | -       | Denotes the beginning of the time period to fetch transactions for. Defaults to beginning of current month. Required if end_date exists. Format: YYYY-MM-DD. |
| end_date          | string  | false    | -       | Denotes the end of the time period you'd like to get transactions for. Defaults to end of current month. Required if start_date exists. Format: YYYY-MM-DD.  |
| debit_as_negative | boolean | false    | false   | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts. Defaults to false.                                   |
| pending           | boolean | false    | false   | Pass in true if you’d like to include imported transactions with a pending status.                                                                           |
| offset            | number  | false    | -       | Sets the offset for the records returned                                                                                                                     |
| limit             | number  | false    | 1000    | Sets the maximum number of records to return.                                                                                                                |
| group_id          | number  | false    | -       | DEPRECATED (Use [GET /v1/transactions-group](#get-transaction-group) instead)                                                                                |

## Get Single Transaction

Use this endpoint to retrieve details about a specific transaction by ID.

> Example 200 Response

```json
{
  "id": 480887173,
  "date": "2023-11-29",
  "amount": "-14.1800",
  "currency": "usd",
  "to_base": -14.18,
  "payee": "Walmart",
  "category_id": 315295,
  "category_name": "Health, Medical",
  "category_group_id": 315357,
  "category_group_name": "Personal",
  "is_income": false,
  "exclude_from_budget": false,
  "exclude_from_totals": false,
  "created_at": "2023-11-30T22:10:57.820Z",
  "updated_at": "2023-11-30T23:59:56.587Z",
  "status": "cleared",
  "is_pending": false,
  "notes": null,
  "original_name": "Walmart",
  "recurring_id": null,
  "recurring_payee": null,
  "recurring_description": null,
  "recurring_cadence": null,
  "recurring_type": null,
  "recurring_amount": null,
  "recurring_currency": null,
  "parent_id": null,
  "has_children": null,
  "group_id": 481307164,
  "is_group": false,
  "asset_id": null,
  "asset_institution_name": null,
  "asset_name": null,
  "asset_display_name": null,
  "asset_status": null,
  "plaid_account_id": 54174,
  "plaid_account_name": "Amex 1002",
  "plaid_account_mask": "1005",
  "institution_name": "American Express",
  "plaid_account_display_name": "Amex Plat",
  "plaid_metadata": "{\"account_id\":\"fMKfypkyRXSXvpJor4vPTg6OP7wD4afmEjv6N\",\"account_owner\":\"1005\",\"amount\":-14.18,\"authorized_date\":\"2023-11-28\",\"authorized_datetime\":null,\"category\":[\"Shops\",\"Supermarkets and Groceries\"],\"category_id\":\"19047000\",\"check_number\":null,\"counterparties\":[{\"confidence_level\":\"VERY_HIGH\",\"entity_id\":\"O5W5j4dN9OR3E6ypQmjdkWZZRoXEzVMz2ByWM\",\"logo_url\":\"https://plaid-merchant-logos.plaid.com/walmart_1100.png\",\"name\":\"Walmart\",\"type\":\"merchant\",\"website\":\"walmart.com\"}],\"date\":\"2023-11-29\",\"datetime\":null,\"iso_currency_code\":\"USD\",\"location\":{\"address\":null,\"city\":null,\"country\":null,\"lat\":null,\"lon\":null,\"postal_code\":null,\"region\":null,\"store_number\":null},\"logo_url\":\"https://plaid-merchant-logos.plaid.com/walmart_1100.png\",\"merchant_entity_id\":\"O5W5j4dN9OR3E6ypQmjdkWZZRoXEzVMz2ByWM\",\"merchant_name\":\"Walmart\",\"name\":\"Walmart\",\"payment_channel\":\"other\",\"payment_meta\":{\"by_order_of\":null,\"payee\":null,\"payer\":null,\"payment_method\":null,\"payment_processor\":null,\"ppd_id\":null,\"reason\":null,\"reference_number\":\"320233330735688096\"},\"pending\":false,\"pending_transaction_id\":null,\"personal_finance_category\":{\"confidence_level\":\"VERY_HIGH\",\"detailed\":\"GENERAL_MERCHANDISE_SUPERSTORES\",\"primary\":\"GENERAL_MERCHANDISE\"},\"personal_finance_category_icon_url\":\"https://plaid-category-icons.plaid.com/PFC_GENERAL_MERCHANDISE.png\",\"transaction_code\":null,\"transaction_id\":\"rmQdnefvAndbfHN5mZ4y703C3vdjk7mozCw1OarL\",\"transaction_type\":\"place\",\"unofficial_currency_code\":null,\"website\":\"walmart.com\"}",
  "plaid_category": "GENERAL_MERCHANDISE_SUPERSTORES",
  "source": "plaid",
  "display_name": "Walmart",
  "display_notes": null,
  "account_display_name": "Amex Plat",
  "tags": [],
  "external_id": null
}
```

> Example 404 Response

```json
{ "error": "Transaction ID not found." }
```

Returns a single Transaction object

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions/:transaction_id`

### Query Parameters

| Parameter         | Type    | Required | Default | Description                                                                                                                |
| ----------------- | ------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------- |
| debit_as_negative | boolean | false    | false   | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts. Defaults to false. |

## Insert Transactions

Use this endpoint to insert many transactions at once.

> Example 200 Response
>
> Upon success, IDs of inserted transactions will be returned in an array.

```json
{
  "ids": [54, 55, 56, 57]
}
```

> Example 404 Response
>
> An array of errors will be returned denoting reason why parameters were deemed invalid.

```json
{
  "error": [
    "Transaction 0 is missing date.",
    "Transaction 0 is missing amount.",
    "Transaction 1 status must be either cleared or uncleared: null"
  ]
}
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/transactions`

### Body Parameters

| Parameter           | Type    | Required | Default | Description                                                                                                                                                      |
| ------------------- | ------- | -------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| transactions        | array   | true     | -       | List of transactions to insert (see below)                                                                                                                       |
| apply_rules         | boolean | false    | false   | If true, will apply account’s existing rules to the inserted transactions. Defaults to false.                                                                    |
| skip_duplicates     | boolean | false    | false   | If true, the system will automatically dedupe based on transaction date, payee and amount. Note that deduping by external_id will occur regardless of this flag. |
| check_for_recurring | boolean | false    | false   | If true, will check new transactions for occurrences of new monthly expenses. Defaults to false.                                                                 |
| debit_as_negative   | boolean | false    | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.                                        |
| skip_balance_update | boolean | false    | true    | If true, will skip updating balance if an asset_id is present for any of the transactions.                                                                       |

### Transaction Object to Insert

| Key          | Type                            | Required | Description                                                                                                                                                                                                                 |
| ------------ | ------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| date         | string                          | true     | Must be in ISO 8601 format (YYYY-MM-DD).                                                                                                                                                                                    |
| amount       | number/string                   | true     | Numeric value of amount. i.e. $4.25 should be denoted as 4.25.                                                                                                                                                              |
| category_id  | number                          | false    | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.                                                                                           |
| payee        | string                          | false    | Max 140 characters                                                                                                                                                                                                          |
| currency     | string                          | false    | Three-letter lowercase currency code in ISO 4217 format. The code sent must exist in our database. Defaults to user account's primary currency.                                                                             |
| asset_id     | number                          | false    | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account.                                                                                                          |
| recurring_id | number                          | false    | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.                                                                                                             |
| notes        | string                          | false    | Max 350 characters                                                                                                                                                                                                          |
| status       | string                          | false    | Must be either cleared or uncleared. (Note: special statuses for recurring items have been deprecated.) Defaults to uncleared.                                                                                              |
| external_id  | string                          | false    | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id.                                                                                                          |
| tags         | Array of numbers and/or strings | false    | Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown. Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created. |

## Update Transaction

Use this endpoint to update a single transaction. You may also use this to split an existing transaction.

> Example 200 Response
>
> If a split was part of the request, an array of newly-created split transactions will be returned.

```json
{
  "updated": true,
  "split": [58, 59]
}
```

> Example 404 Response
>
> An array of errors will be returned denoting reason why parameters were deemed invalid.

```json
{ "error": ["This transaction doesn't exist or you don't have access to it."] }
```

```json
{
  "error": [
    "You cannot change the amount for this transaction because it was automatically imported.",
    "You cannot assign an asset_id for this transaction because it was automatically imported."
  ]
}
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/transactions/:transaction_id`

### Body Parameters

| Parameter           | Type    | Required        | Default | Description                                                                                                                                                                                                                                                   |
| ------------------- | ------- | --------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| split               | Split[] | see description | -       | Defines the split of a transaction. You may not split an already-split transaction, recurring transaction, or group transaction. (see Split object below). If passing an array of split objects to this parameter, the transaction parameter is not required. |
| transaction         | object  | see description | -       | The transaction update object (see Update Transaction object below). Must include an `id` matching an existing transaction. If passing a transaction object to this parameter, the split parameter is not required.                                           |
| debit_as_negative   | boolean | false           | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.                                                                                                                                     |
| skip_balance_update | boolean | false           | true    | If false, will skip updating balance if an asset_id is present for any of the transactions.                                                                                                                                                                   |

### Update Transaction Object

| Key          | Type                            | Description                                                                                                                                                                                                                                                                                                     |
| ------------ | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| date         | string                          | Must be in ISO 8601 format (YYYY-MM-DD).                                                                                                                                                                                                                                                                        |
| category_id  | number                          | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.                                                                                                                                                                               |
| payee        | string                          | Max 140 characters                                                                                                                                                                                                                                                                                              |
| amount       | number or string                | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id                                                                                                                                                       |
| currency     | string                          | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id. Defaults to user account's primary currency.                                                                                                         |
| asset_id     | number                          | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id                                    |
| recurring_id | number                          | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.                                                                                                                                                                                                 |
| notes        | string                          | Max 350 characters                                                                                                                                                                                                                                                                                              |
| status       | string                          | Must be either cleared or uncleared. Defaults to uncleared. (Note: special statuses for recurring items have been deprecated.) Defaults to uncleared.                                                                                                                                                           |
| external_id  | string                          | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id                                    |
| tags         | array of numbers and/or strings | Input must be an array, or error will be thrown. Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown. Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created. Pass in `null` to remove all tags. |

### Split Object

| Key         | Type          | Required | Description                                                                                                                                |
| ----------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| payee       | string        | false    | Max 140 characters. Sets to original payee if none defined                                                                                 |
| date        | string        | false    | Must be in ISO 8601 format (YYYY-MM-DD). Sets to original date if none defined                                                             |
| category_id | number        | false    | Unique identifier for associated category_id. Category must be associated with the same account. Sets to original category if none defined |
| notes       | string        | false    | Sets to original notes if none defined                                                                                                     |
| amount      | number/string | true     | Individual amount of split. Currency will inherit from parent transaction. All amounts must sum up to parent transaction amount.           |

## Unsplit Transactions

Use this endpoint to unsplit one or more transactions.

> Example 200 Response

```json
[84389, 23212, 43333]
```

> Example 404 Response

```json
{
  "error": "The following transaction ids are not valid to unsplit: 1232, 1233, 1234"
}
```

Returns an array of IDs of deleted transactions

### HTTP Request

`POST https://dev.lunchmoney.app/v1/transactions/unsplit`

### Body Parameters

| Parameter      | Type             | Required | Default | Description                                                                                              |
| -------------- | ---------------- | -------- | ------- | -------------------------------------------------------------------------------------------------------- |
| parent_ids     | array of numbers | true     | -       | Array of transaction IDs to unsplit. If one transaction is unsplittable, no transaction will be unsplit. |
| remove_parents | boolean          | false    | false   | If true, deletes the original parent transaction as well. Note, this is unreversable!                    |

## Get Transaction Group

Use this endpoint to get the parent transaction and associated children transactions of a transaction group.

> Example 200 Response

```json
{
  "id": 481307164,
  "date": "2023-11-29",
  "amount": "0",
  "currency": "usd",
  "to_base": 0,
  "payee": "Walmart+",
  "category_id": 315174,
  "category_name": "General",
  "category_group_id": 315362,
  "category_group_name": "Shopping",
  "is_income": false,
  "exclude_from_budget": false,
  "exclude_from_totals": false,
  "created_at": "2023-11-30T23:59:56.584Z",
  "updated_at": "2023-11-30T23:59:56.584Z",
  "status": "cleared",
  "is_pending": false,
  "notes": null,
  "original_name": null,
  "recurring_id": null,
  "recurring_payee": null,
  "recurring_description": null,
  "recurring_cadence": null,
  "recurring_type": null,
  "recurring_amount": null,
  "recurring_currency": null,
  "parent_id": null,
  "has_children": null,
  "group_id": null,
  "is_group": true,
  "asset_id": null,
  "asset_institution_name": null,
  "asset_name": null,
  "asset_display_name": null,
  "asset_status": null,
  "plaid_account_id": null,
  "plaid_account_name": null,
  "plaid_account_mask": null,
  "institution_name": null,
  "plaid_account_display_name": null,
  "plaid_metadata": null,
  "plaid_category": null,
  "source": null,
  "display_name": "Walmart+",
  "display_notes": null,
  "account_display_name": " ",
  "tags": [],
  "children": [
    {
      "id": 480887173,
      "payee": "Walmart",
      "amount": "-14.1800",
      "currency": "usd",
      "date": "2023-11-29",
      "formatted_date": "2023-11-29",
      "notes": null,
      "asset_id": null,
      "plaid_account_id": 54174,
      "to_base": -14.18
    },
    {
      "id": 480887180,
      "payee": "Walmart",
      "amount": "14.1800",
      "currency": "usd",
      "date": "2023-11-28",
      "formatted_date": "2023-11-28",
      "notes": null,
      "asset_id": null,
      "plaid_account_id": 54174,
      "to_base": 14.18
    }
  ],
  "external_id": null
}
```

> Example 404 Response

```json
{
  "error": [
    "Transaction 35360525 is not a transaction group, or part of a transaction group."
  ]
}
```

Returns the hydrated parent transaction of a transaction group

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions/group`

### Query Parameters

| Parameter      | Type   | Required | Description                                                                         |
| -------------- | ------ | -------- | ----------------------------------------------------------------------------------- |
| transaction_id | number | true     | Transaction ID of either the parent or any of the children in the transaction group |

## Create Transaction Group

Use this endpoint to create a transaction group of two or more transactions.

> Example 200 Response

```json
84389
```

> Example 404 Response

```json
{
  "error": [
    "Transaction 35360525 is in a transaction group already (35717487) and cannot be added to another transaction group."
  ]
}
```

Returns the ID of the newly created transaction group

### HTTP Request

`POST https://dev.lunchmoney.app/v1/transactions/group`

### Body Parameters

| Parameter    | Type   | Required | Default | Description                                                  |
| ------------ | ------ | -------- | ------- | ------------------------------------------------------------ |
| date         | string | true     | -       | Date for the grouped transaction                             |
| payee        | string | true     | -       | Payee name for the grouped transaction                       |
| category_id  | number | false    | -       | Category for the grouped transaction                         |
| notes        | string | false    | -       | Notes for the grouped transaction                            |
| tags         | array  | false    | -       | Array of tag IDs for the grouped transaction                 |
| transactions | array  | true     | -       | Array of transaction IDs to be part of the transaction group |

## Delete Transaction Group

Use this endpoint to delete a transaction group. The transactions within the group will not be removed.

> Example 200 Response

```json
{ "transactions": [121232, 324324, 545455] }
```

> Example 404 Response

```json
{ "error": ["No transactions found for this group_id 35717487."] }
```

Returns the IDs of the transactions that were part of the deleted group

### HTTP Request

`DELETE https://dev.lunchmoney.app/v1/transactions/group/:transaction_id`

---
