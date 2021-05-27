# Crypto

## Crypto Object
Attribute Name      | Type   | Description
------------------- | ----   | -----------
id                  | number | Unique identifier for a manual crypto account (no ID for synced accounts)
zabo_account_id     | number | Unique identifier for a synced crypto account (no ID for manual accounts, multiple currencies may have the same zabo_account_id)
source              | string | One of:<br/><ul><li>`synced`: this account is synced via a wallet, exchange, etc.</li><li>`manual`: this account balance is managed manually</li></ul>
name                | string | Name of the crypto asset
display_name        | string | Display name of the crypto asset (as set by user)
balance             | string | Current balance
balance_as_of       | string | Date/time the balance was last updated in ISO 8601 extended format
currency            | string | Abbreviation for the cryptocurrency
status              | string | The current status of the crypto account. Either active or in error.
institution_name    | string | Name of provider holding the asset
created_at          | string | Date/time the asset was created in ISO 8601 extended format

## Get All Crypto
Use this endpoint to get a list of all cryptocurrency assets associated with the user's account. Both crypto balances from synced and manual accounts will be returned.

> Example 200 Response

```json
{
    "crypto": [
        {
            "zabo_account_id": 544,
            "source": "synced",
            "created_at": "2020-07-27T11:53:02.722Z",
            "name": "Dogecoin",
            "display_name": null,
            "balance": "1.902383849000000000",
            "balance_as_of": "2021-05-21T00:05:36.000Z",
            "currency": "doge",
            "status": "active",
            "institution_name": "MetaMask"
        },
        {
            "id": 152,
            "source": "manual",
            "created_at": "2021-04-03T04:16:48.230Z",
            "name": "Ether",
            "display_name": "BlockFi - ETH",
            "balance": "5.391445130000000000",
            "balance_as_of": "2021-05-20T16:57:00.000Z",
            "currency": "ETH",
            "status": "active",
            "institution_name": "BlockFi"
        },
    ]
}
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

> Example Error Response (sends as 200)

```json
{ "errors": [ "currency is invalid for crypto: fakecoin. It may not be supported yet. Request to get it supported via the app or support@lunchmoney.app" ] }
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
