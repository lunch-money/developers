# Transactions

## Transaction Object
Attribute Name   | Type    | Description
---------------- | ------- | -----------
id               | number  | Unique identifier for transaction
date             | string  | Date of transaction in ISO 8601 format
payee            | string  | Name of payee If recurring_id is not null, this field will show the payee of associated recurring expense instead of the original transaction payee
amount           | string  | Amount of the transaction in numeric format to 4 decimal places
currency         | string  | Three-letter lowercase currency code of the transaction in ISO 4217 format
notes            | string  | User-entered transaction notes If recurring_id is not null, this field will be description of associated recurring expense
category_id      | number  | Unique identifier of associated category (see Categories)
asset_id         | number  | Unique identifier of associated manually-managed account (see Assets) Note: plaid_account_id and asset_id cannot both exist for a transaction
plaid_account_id | number  | Unique identifier of associated Plaid account (see Plaid Accounts) Note: plaid_account_id and asset_id cannot both exist for a transaction
status           | string  | One of the following: cleared: User has reviewed the transaction uncleared: User has not yet reviewed the transaction recurring: Transaction is linked to a recurring expense recurring_suggested: Transaction is listed as a suggested transaction for an existing recurring expense. User intervention is required to change this to recurring.
parent_id        | number  | Exists if this is a split transaction. Denotes the transaction ID of the original transaction. Note that the parent transaction is not returned in this call.
is_group         | boolean | True if this transaction represents a group of transactions. If so, amount and currency represent the totalled amount of transactions bearing this transaction’s id as their group_id. Amount is calculated based on the user’s primary currency.
group_id         | number  | Exists if this transaction is part of a group. Denotes the parent’s transaction ID
tags             | Tag[]   | Array of Tag objects
external_id      | string  | User-defined external ID for any manually-entered or imported transaction. External ID cannot be accessed or changed for Plaid-imported transactions. External ID must be unique by asset_id. Max 75 characters.

## Get All Transactions

Use this endpoint to retrieve all transactions between a date range.

> Example 200 Response

```json
{
  "transactions": [
    {
      "id": 602,
      "date": "2020-01-01",
      "payee": "Starbucks",
      "amount": "4.5000",
      "currency": "cad",
      "notes": "Frappuccino",
      "category_id": null,
      "recurring_id": null,
      "asset_id": null,
      "plaid_account_id": null,
      "status": "cleared",
      "is_group": false,
      "group_id": null,
      "parent_id": null,
      "external_id": null
    },
    {
      "id": 603,
      "date": "2020-01-02",
      "payee": "Walmart",
      "amount": "20.9100",
      "currency": "usd",
      "notes": null,
      "category_id": null,
      "recurring_id": null,
      "asset_id": 153,
      "plaid_account_id": null,
      "status": "uncleared",
      "is_group": false,
      "group_id": null,
      "parent_id": null,
      "external_id": "jf2r3t98o943"
    }
  ]
}
```

> Example 404 Response

```json
{ "error": "Both start_date and end_date must be specified." }
```

Returns list of Transaction objects. If no query parameters are set, this endpoint will return transactions for the current calendar month (see `start_date` and `end_date`)

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions`

### Query Parameters

| Parameter         | Type    | Required | Default | Description                                                                                                                                                                                                                                                                  |
| ----------------- | ------- | -------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tag_id            | number  | false    | -       | Filter by tag. Only accepts IDs, not names.
| recurring_id      | number  | false    | -       | Filter by recurring expense
| plaid_account_id  | number  | false    | -       | Filter by Plaid account
| category_id       | number  | false    | -       | Filter by category. Will also match category groups.
| asset_id          | number  | false    | -       | Filter by asset
| offset            | number  | false    | -       | Sets the offset for the records returned
| limit             | number  | false    | -       | Sets the maximum number of records to return. **Note:** The server will not respond with any indication that there are more records to be returned. Please check the response length to determine if you should make another call with an offset to fetch more transactions.
| start_date        | string  | false    | -       | Denotes the beginning of the time period to fetch transactions for. Defaults to beginning of current month. Required if end_date exists. Format: YYYY-MM-DD.
| end_date          | string  | false    | -       | Denotes the end of the time period you'd like to get transactions for. Defaults to end of current month. Required if start_date exists. Format: YYYY-MM-DD.
| debit_as_negative | boolean | false    | false   | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts. Defaults to false.

## Get Single Transaction

Use this endpoint to retrieve details about a specific transaction by ID.

> Example 200 Response

```json
{
    "id": 31,
    "date": "2019-02-04",
    "payee": "Shell",
    "amount": "960.0000",
    "currency": "jpy",
    "notes": null,
    "category_id": 22,
    "recurring_id": null,
    "asset_id": null,
    "plaid_account_id": null,
    "status": "cleared",
    "is_group": false,
    "group_id": null,
    "parent_id": null,
    "has_children": null,
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

Parameter         | Type    | Required | Default | Description
---------         | ----    | -------- | ------- | -----------
debit_as_negative | boolean | false    |  false  | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts. Defaults to false.

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
{ "error":
   [ "Transaction 0 is missing date.",
     "Transaction 0 is missing amount.",
     "Transaction 1 status must be either cleared or uncleared: null" ] }
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/transactions`

### Body Parameters
Parameter           | Type    | Required | Default | Description
---------           | ----    | -------- | ------- | -----------
transactions        | array   | true     | -       | List of transactions to insert (see below)
apply_rules         | boolean | false    | false   | If true, will apply account’s existing rules to the inserted transactions. Defaults to false.
skip_duplicates     | boolean | false    | false   | If true, the system will automatically dedupe based on transaction date, payee and amount. Note that deduping by external_id will occur regardless of this flag.
check_for_recurring | boolean | false    | false   | If true, will check new transactions for occurrences of new monthly expenses. Defaults to false.
debit_as_negative   | boolean | false    | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.


### Transaction Object to Insert
Key          | Type                            | Required | Description
---          | ----                            | -------- | -----------
date         | string                          | true     | Must be in ISO 8601 format (YYYY-MM-DD).
amount       | number/string                   | true     | Numeric value of amount. i.e. $4.25 should be denoted as 4.25.
category_id  | number                          | false    | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.
payee        | string                          | false    | Max 140 characters
currency     | string                          | false    | Three-letter lowercase currency code in ISO 4217 format. The code sent must exist in our database. Defaults to user account's primary currency.
asset_id     | number                          | false    | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account.
recurring_id | number                          | false    | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.
notes        | string                          | false    | Max 350 characters
status       | string                          | false    | Must be either cleared or uncleared. If recurring_id is provided, the status will automatically be set to recurring or recurring_suggested depending on the type of recurring_id.  Defaults to uncleared.
external_id  | string                          | false    | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id.
tags         | Array of numbers and/or strings | false    | Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown. Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created.

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
{ "error":
   [ "This transaction doesn't exist or you don't have access to it." ] }
```
```json
{ "error":
   [ "You cannot change the amount for this transaction because it was automatically imported.",
     "You cannot assign an asset_id for this transaction because it was automatically imported." ] }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/transactions/:transaction_id`

### Body Parameters
Parameter         | Type    | Required | Default | Description
---------         | ----    | -------- | ------- | -----------
split             | object  | false    | -       | Defines the split of a transaction. You may not split an already-split transaction, recurring transaction, or group transaction. (see Split object below)
transaction       | object  | true     | -       | Updates to transaction matching ID (see Update Transaction object below)
debit_as_negative | boolean | false    | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.

### Update Transaction Object
Key          | Type                            | Description
---          | ----                            | -----------
date         | string                          | Must be in ISO 8601 format (YYYY-MM-DD).
category_id  | number                          | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.
payee        | string                          | Max 140 characters
amount       | number or  string               | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
currency     | string                          | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id. Defaults to user account's primary currency.
asset_id     | number                          | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
recurring_id | number                          | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.
notes        | string                          | Max 350 characters
status       | string                          | Must be either cleared or uncleared. Defaults to uncleared If recurring_id is provided, the status will automatically be set to recurring or recurring_suggested depending on the type of recurring_id. Defaults to uncleared.
external_id  | string                          | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
tags         | array of numbers and/or strings | Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown.  Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created.


### Split Object
Key         | Type          | Required | Description
---         | ----          | -------- | -----------
date        | string        | true     | Must be in ISO 8601 format (YYYY-MM-DD).
category_id | number        | true     | Unique identifier for associated category_id. Category must be associated with the same account.
notes       | string        | false    |
amount      | number/string | true     | Individual amount of split. Currency will inherit from parent transaction. All amounts must sum up to parent transaction amount.

---
