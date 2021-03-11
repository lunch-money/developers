# Getting Started

Welcome to the Lunch Money developer API! We created this to enable the user and the community to build rich plug-ins to complement their Lunch Money experience.

---

## Current Status

The developer API is officially in open public beta. During this time, please continue to heed caution and use this API at your own risk as any and all changes are irreversible.

We welcome feedback via email (support@lunchmoney.app). These docs are also on [Github](https://github.com/lunch-money/developers), so if you see a mistake or something that could be improved, feel free to open a pull request!

## Connecting to the Lunch Money API

### Connect to the server

The Lunch Money API endpoint is: `https://dev.lunchmoney.app`

<aside class="notice">
For POST requests, ensure you set Content-Type to application/json
</aside>


## Authentication

> Use Bearer Tokens in your requests like this:

> Recommended

```text
https://dev.lunchmoney.app/v1/categories

with the Authorization header field set to Bearer YOURACCESSTOKEN
```

> Not Recommended

```text
https://dev.lunchmoney.app/v1/categories?access_token=YOURACCESSTOKEN
```

Lunch Money API requests are authenticated using the Bearer Token authentication method.

### Getting an access token

Get your access token by going to [the developers page in the Lunch Money app](https://my.lunchmoney.app/developers).

## What should I build?

**Great question!** We’ve tried to expose the minimum endpoints needed to enable you to build powerful products and extensions.

Here are a few ideas of what you can build:

### Basic use cases

**Integration with your bank**

Does your bank offer an API? You can build a bridge between Lunch Money and your bank to import transactions automatically.

**Sync Lunch Money data to your personal interface**

If you have your own spreadsheet or other interface, use the API to sync data for personalized viewing and analytics.

### Specific use cases with high demand:

**Amazon receipt matcher**

Do you make a lot of Amazon purchases? I'm sure by now you know how frustrating it is to try to identify exactly what the expense is for! Build an Amazon receipt matcher that pulls in your Amazon purchase history and matches on transactions in Lunch Money and updates the notes.

<aside class="notice">
Note: apparently Amazon’s API doesn't expose consumer transactions, so to achieve this, you may need to employ another method of getting transaction details (ideas: email forwarder, chrome extension/screen scraper…)
</aside>

**Venmo integration via email**

The lack of a Venmo and Plaid integration is frustrating for many of our users and is largely out of our control.  Since Venmo sends an email notification every time you send or receive money, build a service that enables users to forward their Venmo notification emails for parsing and insertion into Lunch Money!

**Companion mobile app**

Build a simple mobile app that allows quick insertion of transactions into your Lunch Money account or quick reviewing.

**Zillow integration**

Create an integration that automatically updates the value of a real estate property in Lunch Money.

---
