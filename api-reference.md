# API v2 Endpoint Reference Details

## 1. Create App-scoped User
This API enables seamless integration between your app and the Cubid ecosystem by managing user identity creation and association. It automatically creates a new app-user (`user_id`) if one does not already exist, and links this user to a Cubid identity stamp based on their provided authentication (e.g., email, phone). Additionally, it returns whether the app-user was newly created ()`is_new_app_user`) and whether this is the first association for the Cubid user (`is_first_cubid_user`), helping to detect potential Sybil attacks.

### Purpose:

Typically use this upon a new user sucessfully authenticating and either starting or completing new user registration with your app. Also use when an existing user returns which you hadn't previously registered with CUBID.

Creating a `user_id` for each user is the entry port to accessing CUBID's services. The CUBID `user_id` can exist alongside your normal user identifier, or can replace it. But if proof-of-personhood, Sybil protection and/or bot-defense is a core requirement upon which your business logic depends, then it is recommended that you use the `user_id` from CUBID as your core user identifier thoughout your app. Doing so will minimize the logic you would otherwise have to build to handle account blacklisting, un-blacklisting, merging, de-authorization and removal. Examples of use cases where this applies include:
- 1-person-1-vote apps
- fairdrop dApps
- learn-to-earn apps
- quadratic funding apps
- quadratic voting apps
- universal basic income

### Endpoint:
`POST /api/v2/create_user`

### What It Does:
This API does the following: 
- creates a new app-user (`user_id`) for your App if none exists
- creates a new cubid-user if none existed
- creates a stamp for the provided auth identity (e.g. email, phone) in CUBID if it didn't already exist
- associates the new app-user with the stamp
- returns the `user_id` along with two booleans indicating 
  - if we had to create a new app-user or if it already existed `is_new_app_user` (which you should presumably already know)
  - if this is the first app-user for this cubid-user, or if it's a subsequent one `is_first_cubid_user` (i.e. a Sybil attack, which you wouldn't have known otherwise)

### Request Parameters:

| Parameter      | Type                              | Required | Description                                                        | Example                                |
|----------------|-----------------------------------|----------|--------------------------------------------------------------------|----------------------------------------|
| apikey         | UUIDv4                            | Yes      | The API key for authentication.                                    | 123e4567-e89b-12d3-a456-426614174000   |
| dapp_id        | UUIDv4                            | Yes      | The ID of the App making the request.                             | 987e6543-e21c-65b4-c321-012345678900   |
| email          | string                            | (*)      | User's email.                                                      | user@example.com                       |
| phone          | uint                              | (*)      | User's phone number, including country code but without "+" sign.  | 14155552671                            |
| evm_account    | string, 42 char, starting with 0x | (*)      | User's EVM-compatible public key.                                  | 0x1234567890abcdef1234567890abcdef12345678 |
| github_sub     | string                            | (*)      | User's unique identifier (sub) from GitHub.                        | 98765432                               |
| twitter_sub    | string                            | (*)      | User's unique identifier (sub) from Twitter.                       | 123456789                              |
| google_sub     | string, 21 char                   | (*)      | User's unique identifier (sub) from Google.                        | 115789234567890123456                  |
| linkedin_sub   | string                            | (*)      | User's unique identifier (sub) from LinkedIn.                      | ABCDEFGHIJKLMNOP                       |
| evm_account    | string, 42 char, starting with 0x | (*)      | User's EVM-compatible public key.                                  | 0xabcdef1234567890abcdef1234567890abcdef12 |

*) One (and only one) identifier is required, across all the available options.

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "dapp_id": "987e6543-e21c-65b4-c321-012345678900",
  "email": "user@example.com"
}
```

### Response:

| Field      | Type    | Description                                     |
|------------|---------|-------------------------------------------------|
| user_id    | UUIDv4  | Unique identifier for the dapp user.            |
| is_new_app_user | Boolean | Indicates if a new user was created for our app.|
| is_first_cubid_user | Boolean | Indicates if this is the first time this human (=cubid-user) appears in your app (=`true`), or if it a portential Sybil attack (=`false`).|
| error      | String  | Error message if something goes wrong.          |

Example (first call / new user):
```
{
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb",
  "is_new_app_user": true,
  "is_first_cubid_user": true,
  "error": null
}
```

Example (second call / existing user):
```
{
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb",
  "new_user": false,
  "error": null
}
```

### Notes:
- Feel free to approach us with suggestions for other `stamp_types` you'd like to see supported.
- `newuser` indicates if the user was new within your App scope. It does not indicate whether or not the user previously existed within the broader CUBID scope.
- Typical scenario: You received a new user and validated against CUBID. You will receive `newuser` = `true` (independent of whether the user pre-existed in CUBID from other Apps)
- Sybil attack scenario: The user tried to create a second account with your App, which CUBID detected as a reentrancy / Sybil attack. You expected `newuser` = `true` but received `newuser` = `false`. You should build your business logic to handle this case. 
---

## 2. Fetch App-Scoped EVM Public Key

### Purpose:
Generates a net new Ethereum public key (EVM account) for a user to use within your App.

### Endpoint:
`POST /api/v2/pk/fetch_evm_public_key`

### What It Does:
- Generates and stores an Ethereum-compatible account (EVM public / private key pair) for the user.
- Associates the account uniquely with the user and with your App.
- Returns only the public key of the account.
- If called a second time for the same user, then will return the same public key.

### Request Parameters:

| Parameter | Type   | Required | Description                               |
|-----------|--------|----------|-------------------------------------------|
| apikey    | UUIDv4 | Yes      | The API key for authentication.           |
| user_id   | UUIDv4 | Yes      | The unique user identifier.               |

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb"
}
```

### Response:

| Field      | Type   | Description                                      |
|------------|--------|--------------------------------------------------|
| publicKey  | String | The generated Ethereum public key (address).     |
| error      | String | Error message if something goes wrong.           |

Example:
```
{
  "publicKey": "0x1234567890abcdef1234567890abcdef12345678",
  "error": null
}
```

### Notes:
- This API should not be used to retreive a user's other available 3rd party EVM accounts (such as MetaMask Wallet accounts)
- Does not natively enable the user to sign transactions. This function should only be called if you plan on transferring NFTs or FTs to the user without their active on-chain involvement. In other words, you must support gas-less or paymaster transactions on behalf of your users.
- Advanced users will be able to later export the private key to any of their 3rd party wallets, if they so choose.
- This API will only ever generate one key per user per App, so handle it with care. 
  - If test wallets are required, then please use a test App for this purpose.
  - Users also have the ability to connect external accounts (e.g. metamask, proof-of-humanity, BrightID, etc.) to CUBID and expose them to your App. To access such external accounts you need to instead use the `fetch_identity` API call.
- Please talk to us if you need other types of public keys generated (e.g. Cardano, Solana, Near, etc.)

---

## 3. Fetch User Data

### Purpose:
Fetches "soft" (unverified and/or shared with other users) data related to a specific user. Examples could include name, address, profile picture, etc.

### Endpoint:
`POST /api/v2/identity/fetch_user_data`

### What It Does:
- Retrieves user data as authorized by the user for this App.
- Returns stamps and scores associated with the user.

### Request Parameters:

| Parameter | Type   | Required | Description                               |
|-----------|--------|----------|-------------------------------------------|
| apikey    | UUIDv4 | Yes      | The API key for authentication.           |
| user_id   | UUIDv4 | Yes      | The unique user identifier.               |

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb"
}
```

### Response:

| Field         | Type   | Description                                      |
|---------------|--------|--------------------------------------------------|
| user_details    | JSON  | Details of the user.                             |
| error         | String | Error message if something goes wrong.           |

Example:
```
{
  "name": "John Doe",
  "address": "100 Main St, Littletown, ABC123, Norway",
  "country": "Norway",
  "coordinates": {
    "lat": 43.7026816,
    "lon": -79.4230784
  },
  "is_human": "true",
  "error": null
}
```

### Notes
- The content within the response of this API may be added to and/or changed over time.
- The is_human flag signifes if the user is treated within CUBID as a single human user, vs. has registered up front as a group or organization. It does not signify whether the user has subsequently been deemed a real human vs. a Sybil attacker or bot. See thee Blacklisted webhooks for that particular distinction.

---

## 4. Fetch Identity

### Purpose:
Retrieves a user's identity data by fetching available stamps and their unique values for a specific user.

### Endpoint:
`POST /api/v2/identity/fetch_identity`

### What It Does:
- Fetches user stamp data based on their `user_id`.
- Retrieves the unique values for the stamps.

### Request Parameters:

| Parameter | Type   | Required | Description                               |
|-----------|--------|----------|-------------------------------------------|
| apikey    | UUIDv4 | Yes      | The API key for authentication.           |
| user_id   | UUIDv4 | Yes      | The unique user identifier.               |

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb"
}
````

### Response:

| Field         | Type   | Description                                      |
|---------------|--------|--------------------------------------------------|
| stamp_details | Array of JSON | Breakdown of the user’s stamp types, values, and status.  |
| error         | String | Error message if something goes wrong.           |

Example:
```
{
  "stamp_details": [
    {
      "stamp_type": "email",
      "value": "user@example.com",
      "status": "verified"
    },
    {
      "stamp_type": "phone",
      "value": "14155552671",
      "status": "unverified"
    }
  ],
  "error": null
}
```

---

## 5. Fetch Overall Score

### Purpose:
Calculates and returns CUBID's overall score based on the user's identity and engagement data. The score can be desicribed as a probabilistic approach to proof-of-personhood.

### Endpoint:
`POST /api/v2/score/fetch_score`

### What It Does:
- Fetches identity and engagement data for the user.
- Calculates the proof-of-personhood score.

### Request Parameters:

| Parameter | Type   | Required | Description                               |
|-----------|--------|----------|-------------------------------------------|
| apikey    | UUIDv4 | Yes      | The API key for authentication.           |
| user_id   | UUIDv4 | Yes      | The unique user identifier.               |

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb"
}
```

### Response:

| Field               | Type   | Description                                      |
|---------------------|--------|--------------------------------------------------|
| cubid_score | Number | The calculated overall proof-of-personhood score        |
| scoring_schema | uint | Schema identifier           |
| error               | String | Error message if something goes wrong.           |

Example:
```
{
  "cubid_score": 21.53,
  "scoring_schema": 3,
  "error": null
}
```

### Notes
- This API is similar to `/api/v2/score/get_score_details`, with the difference that this API only provides a summary of the score without the details.
- The score is calculated dynamically based on the schema which was selected and defined as part of your App setup in CUBID's Admin Console.
- The scoring logic is cumulative across `stamp_types` but not within the same type. In other words, he same `stamp_type` cannot be scored more than once. For example, if the user has three different `evm_accounts`, then only the account with the highest score will be considered in the score.

---

## 6. Fetch Score Details

### Purpose:
Retrieves the score details for a user by calculating their total score based on their stamps, where the score for each stamp is unique depending on how hard it is to forge.

### Endpoint:
`POST /api/v2/score/fetch_score_details`

### What It Does:
- Fetches and the user’s individual scores for each stamp.
- Provides a breakdown of stamps and associated values.

### Request Parameters:

| Parameter | Type   | Required | Description                               |
|-----------|--------|----------|-------------------------------------------|
| apikey    | UUIDv4 | Yes      | The API key for authentication.           |
| user_id   | UUIDv4 | Yes      | The unique user identifier.               |

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "user_id": "f12e4567-e89b-42d3-a456-326614174bbb"
}
```

### Response:

| Field         | Type   | Description                                      |
|---------------|--------|--------------------------------------------------|
| score_details | Array of JSON  | Breakdown of the user’s stamp types and values.  |
| cubid_score | number | The calculated overall proof-of-personhood score        |
| scoring_schema | uint | Schema identifier           |
| error         | string | Error message if something goes wrong.           |

Example:
```
{
  "score_details": [
    {
      "stamp_type": "email",
      "score_value": 2
    },
    {
      "stamp_type": "phone",
      "score_value": 5
    },
    {
      "stamp_type": "evm_account",
      "score_value": 1.5
    }
  ],
  "cubid_score": 8.5,
  "scoring_schema": 3,
  "error": null
}
```

### Notes
- This API is similar to `/api/v2/score/fetch_score`, with the difference that this API also provides a breakdown of the score into it's components.
- The score is calculated dynamically based on the schema which was selected and defined as part of your App setup in CUBID's Admin Console.
- The scoring logic is cumulative across `stamp_types` but not within the same type. In other words, he same `stamp_type` cannot be scored more than once. For example, if the user has three different `evm_accounts`, then only the account with the highest score will be considered in the score.
