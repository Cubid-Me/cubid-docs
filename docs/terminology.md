# Terminology
This glossary provides definitions for terms and concepts specific to the CUBID API, helping developers and users better understand the language and functionality used in the documentation.

#### **API Key (`apikey`)**
A unique key provided to App developers or administrators to authenticate API requests. The `apikey` is required in most CUBID API requests to ensure that only authorized Apps can interact with the CUBID platform. It helps control access and protect user data.

#### **App**
We use the termn App as a summary term for all 3rd party websites, web-apps, phone apps, dApps, and platforms that interaface directly with CUBID. So, in CUBID's context, an App refers to any application that uses CUBID APIs to manage user identities, permissions, stamps, and wallets.

#### **App ID (`dapp_id`)**
A unique identifier representing a specific App that uses CUBID's services. While in many cases `dapp_id` is implicit in user interactions, it can be used to scope requests and manage resources for specific Apps within the CUBID platform.

#### **EVM Public Key or EVM Account**
An Ethereum Virtual Machine (EVM) Public Key is the public address of an Ethereum wallet associated with a user in the App. It can also be thought of as an EVM Account. The CUBID API can generate and store EVM accounts for users, allowing them to interact with Ethereum-based Apps and perform transactions. 

(Some sites will incorrectly use the term Wallets to describe EVM accounts. Technically speaking a wallet holds the corresponding private keys of one or more EVM Account. In our case, the CUBID App is technically the Wallet.)

#### **Page ID (`page_id`)**
In the context of CUBID APIs, the `page_id` is used to identify specific pages or sections within an App for which certain permissions or stamps are required. This allows the App to control access and ensure only authorized users can view or interact with specific content.

#### **Proof-of-Personhood**
Proof-of-personhood is a verification process that ensures a user represents a real individual rather than a bot or duplicate account. CUBID's proof-of-personhood score is derived from stamps and other identity validation methods to give Apps confidence that each user is a unique, verified individual.

#### **Score**
In CUBID, a score refers to the numerical value assigned to a user based on various engagement metrics, stamps, or interactions within the App. Scores can reflect a user's activity level, reputation, or proof-of-personhood within the App ecosystem. This score is calculated using stamps of different weight based on either CUBIDs standard formula or a formula defined specifically for your App. The Score quantifies trustworthiness of the the user’s identity (but not their overall trustworthiness or their standing in the community.)

#### **Session**
A session refers to a period in which a user is authenticated and actively interacting with an App. In CUBID, session-based querying is recommended, meaning Apps retrieve the most up-to-date user data during each authenticated session, rather than storing sensitive data long-term in their back-end.

#### **Stamps**
We refer to unique identifiers for a user as "stamps". Much like stamps in a passport, these data points reveal something about you as a user. Common types of stamps include your email, phone number, or your Facebook/Twitter/LinkedIn account. But it can also be less common identifiers, such as the public keys in your crypto wallet, a proof-of-personhood from BrightID or WorldCoin, and many others. The common denominator for all stamps are that they identify something about you personally. So a residential address for example cannot be a CUBID Stamp since it can be shared with other people.

A stamp is a digital attribute assigned to a user to represent a specific type of validation, identity marker, or proof. Stamps can serve as records for things like verified email addresses, phone numbers, blockchain accounts, Facebook account or any other similar proofs of identity. Stamps are essential in proof-of-personhood systems, providing a trusted method for validating user data and engagement.

The earlies proof-of-personhood system was called Proof-of-Humanity. Their system required users to record a video of themselves with their Ethereum account printed on a piece of paper, intimately and publicly linking you to that "stamp". 

In CUBID, we do the same thing but much better and more privacy-preservint. We allow users to prove themselves not only with Ethereum accounts but many other stamps too. The more stamps you have, the stronger your identity and the higher your score will be. Each stamp is also assigned a weight depending on how hard it is for you to to create a new one.

#### **Stamp Type (`stamp_type`)**
The category or type of stamp that is assigned to a user. Examples of stamp types might include `email`, `phone`, `evm_account`, `facebook_sub`, or other forms of identity validation. Each stamp type carries specific meaning and helps categorize how a user has been validated within an App.

#### **Sub**
A `sub` (subscriber) is a unique identifier typically used within authentication systems such ase OAuth to represent the identity of a user. In CUBID, the `sub` of various social network is used as one of the key identifiers to distinguish Stamps for a user.

CUBID does not use any "sub-equivalent" for our own users. Instead, CUBID uses App-scoped `user_id`, see more on this above.

#### **User ID (`user_id`)**
A user_id in CUBID is a unique, app-scoped identifier specific to each App using the CUBID APIs. This means the same CUBID user will have a different user_id for each App, ensuring privacy by preventing cross-site tracking of users and therby adding significant protection to personal and sensitive information. The user_id is used across the API endpoints to associate a user with their stamps, scores, and identity data, allowing Apps to fetch specific information about users without compromising their privacy.

#### **Wallet**
A Wallet if often mistakenly confused for a blockchain account or private key. We use the term Wallet for an App that manages one or more Public Keys. We also distinguish between 3rd party Wallets vs. our own interal CUBID-wallet:
- Users can connect their 3rd party Wallets (e.g. Metamask) to CUBID in order to sign a transaction proving that they own one or more blockchain accounts. Apps can get access to this validated public key information through our `fetch_identity` API
- Technically speaking CUBID also acts as a Wallet, since it is capable of creating Publick/Private Key Pairs for users. Apps can generate and access these Private Keys through out `fetch_evm_public_key` API
