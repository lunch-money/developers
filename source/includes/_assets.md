# Assets

## Assets Object
Attribute Name      | Type   | Description
------------------- | ----   | -----------
id                  | number | Unique identifier for asset
type_name           | string | Primary type of the account. Must be one of:<br><ul> <li>employee compensation</li> <li>cash</li> <li>vehicle</li> <li>loan</li> <li>cryptocurrency</li> <li>investment</li> <li>other</li> <li>credit</li> <li>real estate</li> </ul>
subtype_name        | string | Optional account subtype. Examples include:<br><ul> <li>retirement</li> <li>checking</li> <li>savings</li> <li>prepaid credit card</li><ul>
name                | string | Name of the asset
balance             | string | Current balance of the account in numeric format to 4 decimal places
balance_as_of       | string | Date/time the balance was last updated in ISO 8601 extended format
currency            | string | Three-letter lowercase currency code of the balance in ISO 4217 format
institution_name    | string | Name of institution holding the account
created_at          | string | Date/time the asset was created in ISO 8601 extended format

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
                "status": "active",
                "institution_name": "Bank of Me",
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
            "status": "active",
            "institution_name": "Bank of You",
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
            "status": "active",
            "institution_name": "Bank of Mom",
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
            "status": "active",
            "institution_name": null,
            "created_at": "2020-01-26T12:27:22.765Z"
        }
    ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/assets`

## Update Asset

Use this endpoint to update a single transaction. You may also use this to split an existing transaction.

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
    "created_at": "2019-08-10T22:46:19.486Z"
}
```

> Example 400 Response

```json
{ "errors": [ "type_name must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation" ] }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/assets/:id`

### Body Parameters
Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
type_name        | string | false    | -       | Must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation
subtype_name     | string | false    | -       | Max 25 characters
name             | string | false    | -       | Max 45 characters
balance          | string | false    | -       | Numeric value of the current balance of the account. Do not include any special characters aside from a decimal point!
balance_as_of    | string | false    | -       | Has no effect if balance is not defined. If balance is defined, but balance_as_of is not supplied or is invalid, current timestamp will be used.
currency         | string | false    | -       | Three-letter lowercase currency in ISO 4217 format. The code sent must exist in our database. Defaults to asset's currency.
institution_name | string | false    | -       | Max 50 characters

---
