# Changelog
A log of changes. Breaking changes will be denoted with 🚨

## May 27, 2021

### New
* New endpoint: [GET /v1/budgets](#get-budget-summary)
* New endpoint: [GET /v1/crypto](#get-all-crypto)
* New endpoint: [PUT /v1/crypto/manual/:id](#update-manual-crypto-asset)

### Changed
* New fields for [GET /v1/assets](#get-all-assets): display_name and closed_on


## March 28, 2020

### New
* Pagination options for [GET /v1/transactions](#get-all-transactions) (limit and offset)
* Filter options for [GET /v1/transactions](#get-all-transactions) (asset_id, recurring_id, plaid_account_id, tag_id, category_id)
* New endpoint: [GET /v1/budget](#get-all-tags)

### Changed
* Support for tags in [PUT /v1/transactions/:id](#update-transaction) and [POST /v1/transactions](#insert-transactions)
* 🚨Split object for [splitting transactions](#update-transaction) is moved out of the Transactions object and to a higher-level. We will still support the split property for a few more weeks before removing it completely.

---
