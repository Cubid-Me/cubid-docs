# API v2 Endpoint Reference Details

## 1. Create App-scoped User

### Purpose:
This API enables seamless integration between your app and the Cubid ecosystem by managing user identity creation and association. It takes an AuthID as input. It then automatically creates a new app-user (`user_id`) if one does not already exist, and links this user to a Cubid identity stamp based on their provided AuthID (e.g., `email`, `phone`). Additionally, it returns whether the app-user was newly created (`is_new_app_user`), whether this is the first association for the Cubid user or the same human posing a a new user (`is_sybil_attack`), and also whether the AuthID was blacklisted (`is_blacklisted`), thereby helping to detect potential Sybil attacks.

### More Info:
Typically use this upon a new user sucessfully authenticating and either starting or completing the new user registration with your app. Also use when an existing user returns which you hadn't previously registered with CUBID.

Creating a `user_id` for each user is the entry port to accessing CUBID's services. The CUBID `user_id` can exist alongside your normal user identifier, or can replace it. If proof-of-personhood, Sybil protection and/or bot-defense is a core requirement upon which your business logic depends, then it is recommended that you use the `user_id` from CUBID as your core user identifier thoughout your app. Doing so will minimize the logic you would otherwise have to build to handle account blacklisting, un-blacklisting, merging, de-authorization and removal. 

Examples of use cases where this applies include:
- 1-person-1-vote apps
- Fairdrop dApps
- Learn-to-earn apps
- Quadratic funding apps
- Quadratic voting apps
- Universal basic income apps

### Endpoint:
`POST /api/v2/create_user`

### What It Does:
This API does the following: 
- creates a new app-user (`user_id`) for your App if none exists
- creates a new cubid-user if none existed
- creates a stamp for the provided AuthID (e.g. email, phone) in CUBID if it didn't already exist
- associates the new app-user with the stamp
- returns the `user_id` along with three booleans: 
  1. `is_new_app_user`: Set to `true` if we had to create a new app-user or `false` if it already existed  (which you would presumably already know, if your app keeps track of attempts)
  2. `is_sybil_attack`: Set to `false` if this is the first app-user for this human / cubid-user, or `true` if it's a subsequent one (i.e. the person had used CUBID elsewhere and disclosed that they own both AuthIDs separately, and is now using them to Sybil attack your protocol)
  3. `is_blacklisted`: Set to `false` if CUBID has no knowledge of misconduct with this AuthID, or set to`true` if this AuthID has already been used in a Sybil Attack elsewhere in the CUBID ecosystem.

### Cases:
- Permissive vs. non-permissive
  - **Permissive**: If `is_permissive` and `is_blacklisted` are both `true` then a `user_id` will be generated. Use this setting if you want to create the app-user and handle the issue within your app.
  - **Non-permissive**: If `is_permissive` is `false` or omitted, and `is_blacklisted` is `true` then no `user_id` will be generated, and an error will be returned. Use this scenario if you plan on throwing out the user immediately or want a simple error to react to.
- Typical vs. Sybil-attack scenarios 
  - **Typical sceneario**: You received a new user and validated against CUBID. You will receive `is_sybil_attack` = `false` and `is_blacklisted` = `false`, indicating all is good
  - **Sybil attack scenario, Internal**: The person is trying to create a second (or third etc.) account within your App, which CUBID detected as a reentrancy into your app and classified as a Sybil attack. You received `is_sybil_attack` = `true`. 
  - **Sybil attack scenario, CUBID-wide**: The user had previously tried to Sybil-attack CUBID as a whole (advertently or inadvertently), by creating two or more CUBID accounts with the same AuthID, which CUBID subsequenlty had detected. You receive `is_blacklisted` = `true`. 



### Request Parameters:

| Parameter      | Type                              | Required | Description                                                        | Example                                |
|----------------|-----------------------------------|----------|--------------------------------------------------------------------|----------------------------------------|
| `apikey`         | UUIDv4                            | Yes      | The API key for authentication.                                    | 123e4567-e89b-12d3-a456-426614174000   |
| `dapp_id`        | UUIDv4                            | Yes      | The ID of the App making the request.                             | 987e6543-e21c-65b4-c321-012345678900   |
| `is_permissive`  | boolean                           | No      | Permissive returns a UUID if the identity is blacklisted. Defaults to non-permissive if omitted, which returns error if the identity is blacklisted.   | TRUE   |
| `email`          | string                            | (*)      | User's email.                                                      | user@example.com                       |
| `phone`          | uint                              | (*)      | User's phone number, including country code but without "+" sign.  | 14155552671                            |
| `evm`            | string, 42 char, starting with 0x | (*)      | User's EVM-compatible public key.                                  | 0x1234567890abcdef1234567890abcdef12345678 |

*) One (and only one) AuthID is required, across all the available options.

> NOTE: This list will expand over time. Refer to the last section of the previous chapter for a longer list of options we are working on enabling.

Example:
```
{
  "apikey": "123e4567-e89b-12d3-a456-426614174000",
  "dapp_id": "987e6543-e21c-65b4-c321-012345678900",
  "email": "user@example.com",
  "is_permissive": true
}
```

### Response:

| Field      | Type    | Description                                     |
|------------|---------|-------------------------------------------------|
| `user_id`    | UUIDv4  | Unique identifier for the dapp user.            |
| `is_new_app_user` | Boolean | Indicates if a new user was created for our app.|
| `is_sybil_attack` | Boolean | Indicates if this is the first time this human (=cubid-user) appears in your app (=`false`), or if it a portential Sybil attack (=`true`).|
| `is_blacklisted` | Boolean | Indicates if the provided AuthID.|
| error      | String  | Error message if something goes wrong.          |

Example (first call, create user):
```
{
  "user_id": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "is_new_app_user": true,
  "is_sybil_attack": false,
  "is_blacklisted": false,
  "error": null
}
```

Example (second call, existing user reentering with same AuthID):
```
{
  "user_id": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "is_new_app_user": **false**,
  "is_sybil_attack": false,
  "is_blacklisted": false,
  "error": null
}
```

Example (user reentering your app with new AuthID, posing as a new user):
```
{
  "user_id": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
  "is_new_app_user": true,
  "is_sybil_attack": **true**,
  "is_blacklisted": false,
  "error": null
}
```

### Notes:
- Feel free to approach us with suggestions for other AuthIdentiy `stamp_types` you'd like to see supported.
- `newuser` indicates if the user was new within your App scope. It does not indicate whether or not the user previously existed within the broader CUBID scope.
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
Retrieves a user's identity data by fetching available stamps and their unique values for a specific user. Each stamp can be shared at various levels
- 1: not shared
- 2: share presence of stamp only as a bool
- 3: share hash of the value
- 4: share the value
- 5: share the full JSON 

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
| stamp_type    | String | Error message if something goes wrong.           |
| share_type    | Int    | 1-5, see above.                                  |
| value         | Depends| Bool, Hash, Sting or JSON depending on share_type|
| status        | String | Verified or Unverified.                          |
| verified_date | Timestamp| Date of last verification. Omitted for unverified.|
| error         | String | Error message if something goes wrong.           |

Example:
```
{
  "stamp_details": [
    {
      "stamp_type": "email",
      "share_type": "4"
      "value": "user@example.com",
      "status": "verified"
      "verified_date": "May 23, 2024, 13:23:55"
    },
    {
      "stamp_type": "phone",
      "share_type": "4"
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

---

## 7. Fetch User's Rough Location

### Purpose:
Retrieves user's rough location based on their `user_id` and the corresponding app’s API key. The endpoint returns details about the user’s latitude, longitude, and country, along with the Google Plus code. The geocoordinates have been rounded to the nearest one decimal places and the Plus Code has been truncated to the first six characters. The accuracy of this location data is roughly within 15km / 9 miles of the user's residential location.

### Endpoint:
`POST /api/v2/identity/fetch_rough_location`

### What It Does:
- Verifies the API key against the `dapps` table.
- Fetches and returns user information if the user is linked to the app through the provided `user_id`.

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

| Field            | Type   | Description                                      |
|------------------|--------|--------------------------------------------------|
| pluscode         | String | The first six characters of Google's Plus code. |
| coordinates      | JSON   | Latitude and longitude with 1 decimal place      |
| country          | String | The user’s country.                              |
| error            | String | Error message if something goes wrong.           |

Example:
```
{
  "pluscode": "7J2X8Q",
  "coordinates": {
    "lat": 30.8,
    "lon": 76.7
  },
  "country": "India",
  "error": null
}
```

### Notes:
- If the API key is invalid, the response will contain a 400 status with an error message: `"Invalid API key"`.
- Location data of the user in this API is NOT validated by CUBID (as opposed to other stamps). The user has an option to state their one residential address, and can only change it once every 6 months. If you need a validated address, please see our paid KYC APIs.

---
## 8. Fetch User's Approximate Location

### Purpose:
Retrieves user's approximate location based on their `user_id` and the corresponding app’s API key. The endpoint returns details such as the user’s latitude, longitude, country, and postal code, along with the Google Plus code and the plain text geographic context (placename) of the pluscode. The geocoordinates have been rounded to the nearest two decimal places, and the postal code has been truncated to the first three digits. Depending on distance from the equator, the accuracy of this location data is roughly 1km / 0.6 miles of the user's actual residential location. (Note that the Pluscode is still 6 characters long in this API, with the lower accuracy of 14km, and the placename has unspecified precision.)

### Endpoint:
`POST /api/v2/identity/fetch_approx_location`

### What It Does:
- Verifies the API key against the `dapps` table.
- Fetches and returns user information if the user is linked to the app through the provided `user_id`.

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

| Field            | Type   | Description                                        |
|------------------|--------|----------------------------------------------------|
| pluscode         | String | The first six characters of Google's Plus code.    |
| placename        | String | The plain text geographic context of the Plus code |
| coordinates      | JSON   | Latitude and longitude with 2 decimal places       |
| country          | String | The user’s country.                                |
| postalcode       | String | The user’s postal code, 3 first digits.            |
| error            | String | Error message if something goes wrong.             |

Example:
```
{
  "pluscode": "59V3H3",
  "placename": "Nesbyen, Norway",
  "coordinates": {
    "lat": 60.47,
    "lon": 8.46
  },
  "country": "Norway",
  "postalcode": "354",
  "error": null
}
```

### Notes:
- If the API key is invalid, the response will contain a 400 status with an error message: `"Invalid API key"`.
- Location data of the user in this API is NOT validated by CUBID (as opposed to other stamps). The user has an option to state their one residential address, and can only change it once every 6 months. If you need a validated address, please see our paid KYC APIs.

---

## 9. Fetch User's Exact Location

### Purpose:
Retrieves user's exact location, as stated by the user, based on their `user_id` and the corresponding app’s API key. The endpoint returns the full details from Google Places API, plus the latitude longitude, country, and postal code, all with full accuracy.

### Endpoint:
`POST /api/v2/identity/fetch_exact_location`

### What It Does:
- Verifies the API key against the `dapps` table.
- Fetches and returns user information if the user is linked to the app through the provided `user_id`.

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

| Field            | Type   | Description                                      |
|------------------|--------|--------------------------------------------------|
| place            | JSON   | Full Google Places information for the location. |
| coordinates      | JSON   | Latitude and longitude with all decimal places   |
| country          | String | The user’s country.                              |
| error            | String | Error message if something goes wrong.           |

Example:
```
{
  "place": {
    "address": "36 Lisgar St, Toronto, ON M6J 0C7, Canada",
    "postcode": "M6j0c7",
    "locationDetails": {
      "business_status": "OPERATIONAL",
      "formatted_address": "36 Lisgar St, Toronto, ON M6J 0C7, Canada",
      "geometry": {
        "location": {
          "lat": 43.6418878,
          "lng": -79.4232449
        },
        "viewport": {
          "northeast": {
            "lat": 43.64305407989271,
            "lng": -79.42179222010728
          },
          "southwest": {
            "lat": 43.63035442010727,
            "lng": -79.23449187989273
          }
        }
      },
      "icon": "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
      "icon_background_color": "#7B9EB0",
      "icon_mask_base_uri": "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
      "name": "Condo",
      "place_id": "ChIJyYzJFo01K4gRW-WfkiUbM44",
      "plus_code": {
        "compound_code": "JHRG+QP Toronto, Ontario, Canada",
        "global_code": "87M2JHRG+QP"
      },
      "rating": 0,
      "reference": "ChIJyYzJFo01K4gRW-WfkiUbM44",
      "types": [
        "point_of_interest",
        "establishment"
      ],
      "user_ratings_total": 0
    },
    "coordinates": {
      "lat": 43.6328906,
      "lon": -79.2562693
    }
  }
  "coordinates": {
    "lat": 43.6328906,
    "lon": -79.2562693
  },
  "country": "Canada",
  "error": null
}
```

### Notes:
- If the API key is invalid, the response will contain a 400 status with an error message: `"Invalid API key"`.
- Location data of the user in this API is NOT validated by CUBID (as opposed to other stamps). The user has an option to state their one residential address, and can only change it once every 6 months. If you need a validated address, please see our paid KYC APIs.
- If the user didn't provide a location then the place may be empty. In such case, Cubid may still have information about the Coordinates, for example if the user allowed Cubid access to their device information. If place is given, then Country would come from there, otherwise Country would be derived from the Coordinates.

