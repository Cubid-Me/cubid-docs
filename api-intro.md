# API v2 Overview
This v2 API documentation simplifies and enhances consistency across endpoints while maintaining robust security and functionality for both Web2 and Web3 platforms.

## Overview

#### App-scoped User IDs
CUBID always generates an App-scoped `user_id`, which means that the user identifier for a user is different in your App from what it would be in other Apps. We take this approach in order to preserve user privacy across apps, and also to dramatically enhance security. Another benefit of this approach is that we can elliminate the need to include `dapp_id` from the API calls in most cases.

#### API Keys vs App IDs
In this v2 API, we have streamlined authentication and scoping mechanisms to enhance consistency, requiring only an `apikey` and `user_id` in most cases. This simplification improves security and makes the APIs easier to integrate for developers working across various Apps and web applications.

#### Admin Console
App admins and developers can generate their `apikey`, `page_id` and `dapp_id` in CUBID's **Admin Console** at [https://admin.cubid.me](https://admin.cubid.me/). The console also allows for key rotation as needed.

#### Self-custodial & Opt-in Principles
It is worth noting that the users themselves are the sole custodials of their own data, and are ultimately in full control of what they share with each App. It is possible that they store information in CUBID that they choose to not share with your App. In such cases the APIs would return null values. They may however choose to include such data in an anonymized way, but including it into the underlying data for their Score calculation.
- When a user has data in CUBID from other apps then they must authorize release to your App before the APIs will return any data. 
- Data that your App may have sent to CUBID, or which was created on your behalf within CUBID, will by default be accessible to you through our API queries.

Ask your users to review and authorize release of their data to your App by sending them to CUBID's **Allow Page**. Read more about this at [Cubid.me](https://cubid.me) 

#### Data Validation & Blacklisting
What CUBID provides is validation that the information that is sent to you has been confirmed to belong to the user themselves. We typically do this by either sending a one-time passcode that we ask the user to validate, or by leverageing OAUTH protocols where possible (e.g. "Sign in with Google"), or by having them connect a web3 wallet if applicable. CUBID does NOT prevent duplicate entry of Stamps across multiple user accounts. Instead, our core logic is designed to detect such attempts and immediately blacklist a Stamp that's been used across more than one user, in what we refer to as a "Sybil attack". 

If for example a user created two CUBID accounts and tried to use the same phone number in both, even if it was for different apps, then that phone number would be blacklisted for both user accounts, and the score would diminish for both.

## Endpoints
*This is an overview. Find the details in the next document "API Reference"*

#### Create User
   - Endpoint: `/api/v2/create_user`
   - Purpose: Creates a new CUBID user for your dapp. Assigns a unique `user_id` identifier to correspond with your own user identifier (e.g. email).

#### Get App-Scoped EVM Public Key
   - Endpoint: `/api/v2/pk/fetch_evm_public_key`
   - Purpose: Generates a new Ethereum wallet (EVM account / public key) for a user.

#### Fetch User Data
   - Endpoint: `/api/v2/identity/fetch_user_data`
   - Purpose: Retrieves detailed data related to a specific user, including stamps and scoring information.

#### Fetch Identity
   - Endpoint: `/api/v2/identity/fetch_identity`
   - Purpose: Retrieves a user's identity details by fetching stamp data for identity verification.

#### Fetch Score Overview
   - Endpoint: `/api/v2/score/fetch_score`
   - Purpose: Fetches and calculates a user's total cumulative score based on their accumulated stamps.

#### Fetch Score Details
   - Endpoint: `/api/v2/score/fetch_score_details`
   - Purpose: Fetches and calculates a user's scores for each of their identity records, as well as the total cumulative score across all of their accumulated stamps.

## Stamptypes Reference
The following list outlines the available non-blockchain stamptypes. C
- Currently we only support email, phone and evm for the `create-user` api. If you are interested in using other stamptypes in this API then please talk to us first.
- All stamps are available for remaining APIs
- For a comprehensive list of blockchain-related stamptypes, refer to the "Blockchain & Crypto" section

| **Category** | *Stamptype** | **Description** | **Oauth Identifier** | **create_user-enabled** |
|---|---|---|---|---|
| blockchain | **evm** | EVM-compatible blockchains |  | **True** |
| kyc | **fractal_basic** | Fractal KYC, Level=Basic |  | False |
| kyc | **fractal_plus** | Fractal KYC, Level=Plus |  | False |
| kyc | **sumsub** | SumSub User Identifier |  | False |
| PII | **email** | Email |  | **True** |
| PII | **phone** | Phone number |  | **True** |
| social | **discord** | Discord Oauth 2.0 | id | (talk to us first) |
| social | **facebook_oauth_id** | Facebook Oauth 2.0 | id | False |
| social | **github_oauth_id** | GitHub Oauth 2.0 - default use case | id | (talk to us first) |
| social | **github_apps** | GitHub for Apps and sometimes for Orgs and Enterprises | id | False |
| social | **google** | Google Oauth 2.0 | sub | (talk to us first) |
| social | **instagram** | Instagram Oauth 2.0 | user_id | False |
| social | **linkedin** | LinkedIn Oauth 2.0 | id | (talk to us first) |
| social | **telegram** | Telegram Oauth 2.0 | id | (talk to us first) |
| social | **twitter** | Twitter Oauth 2.0 | id | (talk to us first) |
| uniqueness | **brightid** | BrightID |  | (talk to us first) |
| uniqueness | **gitcoin_passport** | Gitcoin Passport |  | (talk to us first) |
| uniqueness | **gooddollar** | GoodDollar |  | (talk to us first) |
| uniqueness | **idena** | Idena Protocol |  | (talk to us first) |
| uniqueness | **proof_of_humanity** | Proof Of Humanity |  | (talk to us first) |
| uniqueness | **worldcoin** | Worldcoin |  | (talk to us first) |