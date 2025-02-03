# Changelog

A log of changes. Breaking changes will be denoted with 🚨
## Feb 3, 2024
- The [Update Transaction Object](#update-transaction-object) now accepts a `plaid_account_id` allowing developers to update existing transactions associated with plaid accounts or to move transactions to a plaid account if the "Allow Modifications to Transactions" property is set.
- The bug in the [GET /transactions/{id}](#get-single-transaction) endpoint where the `debit_as_negative` query param was ignored has been fixed. 

## Dec 20, 2024
- The [Transaction Insert Object](#transaction-object-to-insert) now accepts a `plaid_account_id` allowing developers to insert transactions associated with plaid accounts if the "Allow Modifications to Transactions" property is set.
- The bug that prevented users from inserting transactions with the same `asset_id`/`external_id` as a previously deleted transaction has been fixed.

## June 15, 2024
- New endpoint: [GET /v1/recurring_items](#get-recurring-items)
- Deprecated the [GET /v1/recurring_expenses](#get-recurring-expenses) endpoint in favor of the new recurring_items endpoint

## March 27, 2024

### Updated

- 🚨 Deprecated `transaction_id` in [Recurring Expenses Object](#recurring-expenses-object) (temporarily... it will come back in another form)
- 🚨 Deprecated special statuses for transactions (`recurring` and `recurring_suggested`)

## January 18, 2024

### New

- New endpoint: [GET /v1/transactions/group](#get-transaction-group)
- New fields for [Transaction object](#transaction-object) and deprecated query parameter (`group_id`) and fields (`type`, `subtype`, `quantity`, `fee`, `price`)

## December 20, 2023

### New

- New endpoint: [POST /v1/plaid_accounts/fetch](#trigger-fetch-from-plaid) (experimental)

## October 8, 2023

### Updated

- New fields for [GET /v1/categories](#get-all-categories): order
- New option for [GET /v1/categories](#get-all-categories): format (flattened or nested)

## June 22, 2022

### New

- New endpoint: [POST /v1/transactions/unsplit](#unsplit-transactions)

## June 4, 2022

### New

- Lots of new endpoints for [categories](#categories) with support for [category groups](#create-category-group)

## August 31, 2021

### Updated

- New fields for [GET /v1/budgets](#get-budget-summary): config

## June 9, 2021

### New

- New endpoint: [POST /v1/transactions/group](#create-transaction-group)
- New endpoint: [DELETE /v1/transactions/group/:transaction_id](#delete-transaction-group)

### Updated

- New option for [POST /v1/transactions](#insert-transactions) and [PUT /v1/transactions](#update-transaction): skip_balance_update
- New fields for [GET /v1/transactions](#get-all-transactions): original_name, type, subtype, fees, price, quantity
- New filter options for [GET /v1/transactions](#get-all-transactions): status, is_group, group_id

## May 27, 2021

### New

- New endpoint: [GET /v1/budgets](#get-budget-summary)
- New endpoint: [GET /v1/crypto](#get-all-crypto)
- New endpoint: [PUT /v1/crypto/manual/:id](#update-manual-crypto-asset)

### Updated

- New fields for [GET /v1/assets](#get-all-assets): display_name and closed_on

## March 28, 2020

### New

- Pagination options for [GET /v1/transactions](#get-all-transactions): limit and offset
- Filter options for [GET /v1/transactions](#get-all-transactions): asset_id, recurring_id, plaid_account_id, tag_id, category_id
- New endpoint: [GET /v1/budget](#get-all-tags)

### Updated

- Support for tags in [PUT /v1/transactions/:id](#update-transaction) and [POST /v1/transactions](#insert-transactions)
- 🚨Split object for [splitting transactions](#update-transaction) is moved out of the Transactions object and to a higher-level. We will still support the split property for a few more weeks before removing it completely.

---
