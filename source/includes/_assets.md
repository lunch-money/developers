# Assets

## Assets Object

Attribute Name       | Type    | Nullable | Description
-------------------- | ------- | -------- | ----------------------------------------------------------------------------
id                   | number  | false    | Unique identifier for asset
type_name            | string  | false    | Primary type of the asset. Must be one of:<br><ul> <li>cash</li> <li>credit</li> <li>investment</li> <li>real estate</li> <li>loan</li> <li>vehicle</li> <li>cryptocurrency</li> <li>employee compensation</li> <li>other liability</li> <li>other asset</li> </ul> |
subtype_name         | string  | true     | Optional asset subtype. Examples include:<br><ul> <li>retirement</li> <li>checking</li> <li>savings</li> <li>prepaid credit card</li><ul>
name                 | string  | false    | Name of the asset
display_name         | string  | true     | Display name of the asset (as set by user)
balance              | string  | false    | Current balance of the asset in numeric format to 4 decimal places
balance_as_of        | string  | false    | Date/time the balance was last updated in ISO 8601 extended format
closed_on            | string  | true     | The date this asset was closed (optional)
currency             | string  | false    | Three-letter lowercase currency code of the balance in ISO 4217 format
institution_name     | string  | true     | Name of institution holding the asset
exclude_transactions | boolean | false    | If true, this asset will not show up as an option for assignment when creating transactions manually
created_at           | string  | false    | Date/time the asset was created in ISO 8601 extended format

## Get All Assets

Use this endpoint to get a list of all manually-managed assets associated with the user's account.

> Example 200 Response

```json
{
  "assets": [
    {
      "id": 72,
      "type_name": "cash",
      "subtype_name": "physical cash",
      "name": "Test Asset 1",
      "balance": "1201.0100",
      "balance_as_of": "2020-01-26T12:27:22.000Z",
      "currency": "cad",
      "institution_name": "Bank of Me",
      "exclude_transactions": false,
      "created_at": "2020-01-26T12:27:22.726Z"
    },
    {
      "id": 73,
      "type_name": "credit",
      "subtype_name": "credit card",
      "name": "Test Asset 2",
      "balance": "0.0000",
      "balance_as_of": "2020-01-26T12:27:22.000Z",
      "currency": "usd",
      "institution_name": "Bank of You",
      "exclude_transactions": false,
      "created_at": "2020-01-26T12:27:22.744Z"
    },
    {
      "id": 74,
      "type_name": "vehicle",
      "subtype_name": "automobile",
      "name": "Test Asset 3",
      "balance": "99999999999.0000",
      "balance_as_of": "2020-01-26T12:27:22.000Z",
      "currency": "jpy",
      "institution_name": "Bank of Mom",
      "exclude_transactions": false,
      "created_at": "2020-01-26T12:27:22.755Z"
    },
    {
      "id": 75,
      "type_name": "loan",
      "subtype_name": null,
      "name": "Test Asset 4",
      "balance": "10101010101.0000",
      "balance_as_of": "2020-01-26T12:27:22.000Z",
      "currency": "twd",
      "institution_name": null,
      "exclude_transactions": true,
      "created_at": "2020-01-26T12:27:22.765Z"
    }
  ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/assets`

## Create Asset

Use this endpoint to create a single (manually-managed) asset.

> Example 200 Response

```json
{
  "id": 34061,
  "type_name": "depository",
  "subtype_name": null,
  "name": "My Test Asset",
  "display_name": null,
  "balance": "67.2100",
  "balance_as_of": "2022-05-29T21:35:36.557Z",
  "closed_on": null,
  "created_at": "2022-05-29T21:35:36.564Z",
  "currency": "cad",
  "institution_name": null,
  "exclude_transactions": false
}
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/assets`

### Body Parameters

Parameter            | Type    | Required | Default                 | Description
-------------------- | ------- | -------- | ----------------------- | --------------------------------------------------
type_name            | string  | true     | -                       | Must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation
subtype_name         | string  | false    | -                       | Max 25 characters
name                 | string  | true     | -                       | Max 45 characters
display_name         | string  | false    | -                       | Display name of the asset (as set by user)
balance              | string  | true     | -                       | Numeric value of the current balance of the account. Do not include any special characters aside from a decimal point!
balance_as_of        | string  | false    | Current timestamp       | Has no effect if balance is not defined. If balance is defined, but balance_as_of is not supplied or is invalid, current timestamp will be used.
currency             | string  | false    | User's primary currency | Three-letter lowercase currency in ISO 4217 format. The code sent must exist in our database. Defaults to user's primary currency.
institution_name     | string  | false    | -                       | Max 50 characters
closed_on            | string  | false    | -                       | The date this asset was closed
exclude_transactions | boolean | false    | false                   | If true, this asset will not show up as an option for assignment when creating transactions manually

## Update Asset

Use this endpoint to update a single asset.

> Example 200 Response

```json
{
  "id": 12,
  "type_name": "cash",
  "subtype_name": "savings",
  "name": "TD Savings Account",
  "balance": "28658.5300",
  "balance_as_of": "2020-03-10T05:17:23.856Z",
  "currency": "cad",
  "institution_name": "TD Bank",
  "exclude_transactions": false,
  "created_at": "2019-08-10T22:46:19.486Z"
}
```

> Example Error Response (sends as 200)

```json
{
  "errors": [
    "type_name must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation"
  ]
}
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/assets/:id`

### Body Parameters

Parameter            | Type    | Required | Default                 | Description
-------------------- | ------- | -------- | ----------------------- | --------------------------------------------------
type_name            | string  | false    | -                       | Must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation
subtype_name         | string  | false    | -                       | Max 25 characters
name                 | string  | false    | -                       | Max 45 characters
display_name         | string  | false    | -                       | Display name of the asset (as set by user)
balance              | string  | false    | -                       | Numeric value of the current balance of the account. Do not include any special characters aside from a decimal point!
balance_as_of        | string  | false    | Current timestamp       | Has no effect if balance is not defined. If balance is defined, but balance_as_of is not supplied or is invalid, current timestamp will be used.
currency             | string  | false    | User's primary currency | Three-letter lowercase currency in ISO 4217 format. The code sent must exist in our database. Defaults to user's primary currency.
institution_name     | string  | false    | -                       | Max 50 characters
closed_on            | string  | false    | -                       | The date this asset was closed
exclude_transactions | boolean | false    | false                   | If true, this asset will not show up as an option for assignment when creating transactions manually

---
