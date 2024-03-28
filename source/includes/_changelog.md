# Changelog

A log of changes. Breaking changes will be denoted with 🚨

## March 27, 2024

### Updated

- 🚨 Deprecated `transaction_id` in [Recurring Expenses Object](#recurring-expenses-object) (temporarily... it will come back in another form)
- 🚨 Deprecated special statuses for transactions (`recurring` and `recurring_suggested`)
- New fields for [Transaction object](#transaction-object) and deprecated query parameter (`group_id`) and fields (`type`, `subtype`, `quantity`, `fee`, `price`)

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
