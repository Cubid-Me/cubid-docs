# Blockchain & Crypto
In addition to traditional personal identifiers, CUBID supports the integration of blockchain-specific StampTypes, allowing decentralized identity verification via cryptographic assets. These StampTypes leverage blockchain technology to ensure secure, trustless validation of user identities, utilizing cryptographic methods to prove ownership of blockchain addresses or assets without exposing sensitive information.

At present, the CUBID platform supports **blockchain account ownership verification** as the core blockchain StampType. This method provides a decentralized and secure way to authenticate users based on their control over a blockchain wallet or address.

- **How It Works**: Users are required to sign a message or transaction using their private key, proving ownership of the associated blockchain account. This signature is verified by the dApp, confirming that the user has control over the corresponding wallet address.
- **Use Case**:
  - **Authentication**: This stamp is primarily used to verify the user's identity based on their blockchain wallet, ensuring secure login and interaction with dApps in the CUBID ecosystem.
  - **Sybil Resistance**: Prevents users from creating multiple accounts using the same blockchain address, thereby enhancing the system’s resistance to Sybil attacks. (Though technically speacing, we don't prevent, instead we blacklist after this happens.)

*See the last section for our plans to add more StampTypes.*

## Blockchain Stamptypes
The stamptypes for blockchain addresses or accounts (sometimes incorrectly referred to as "wallets") are separated by type of cryptographic algoritms used to derive the public key, and by output format. Some chains support more than one type (e.g. Bitcoin, Celo and Near), while other types are common across many chains (e.g. EVM).

The bolded identifiers in this table are what we expect to be used in API calls to CUBID.

| Type | **Stamptype** | Format | Example | Example Chains & Protocols |
|---|---|---|---|---|
| Algorand Address | **algorand** | Base32 encoding, 32-byte address | RV4SJLKYFPWQ6H7Y7MBPOOCCHWPZ5ZO6BNGRSGQ | Algorand |
| Aptos Address | **aptos** | 32-byte hexadecimal address, unique to Move VM | 0x1... | Aptos |
| Avalanche X-Chain Address | **avalanche-x** | Bech32 encoding, starts with X-avax | X-avax1zdvr64ch4u34lgj4gy8fr7z0g3nvqe6ldhgytj | Avalanche (X-Chain) |
| Bitcoin (Bech32 / SegWit) | **bitcoin-segwit** | Bech32 encoding, starts with bc1 | bc1qw508d6qejxtdg4y5r3zarvary0c5xw7kygt080 | Bitcoin, Bitcoin Cash |
| Bitcoin (P2PKH) | **bitcoin-p2pkh** | Base58Check encoding, starts with 1 | 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa | Bitcoin, Bitcoin Cash, Bitcoin SV, Dogecoin |
| Bitcoin (P2SH) | **bitcoin-p2sh** | Base58Check encoding, starts with 3 | 3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy | Bitcoin, Bitcoin Cash, Bitcoin SV |
| Cardano (Shelley Address) | **cardano** | Bech32 encoding, starts with addr1 | addr1qxw5uvlfpca2g47ln6djg58tflamgnk25m84kxqjgsnfxnm66yeyu8ev7wtqh6 | Cardano |
| Celo (Mobile Addressing) | **celo-mobile** | Hashing of mobile number to address | +1234567890 -> 0x1234567890abcdef1234567890abcdef12345678 | Celo |
| Chiliz Address | **chiliz** | EVM-compatible, starts with 0x | 0x3506424f91fdc8930d6a85e880227326c9007f | Chiliz |
| Cosmos / Terra Address | **cosmos-terra** | Bech32 encoding, starts with cosmos or terra | cosmos1zt50azupanqj7paz6tnrda5tkpqpgrkquf7kvu | Cosmos, Terra |
| Elrond (eGLD Address) | **elrond** | Bech32 encoding, starts with erd | erd1qqqqqqqqqqqqqpgq9m5qmnw4c7un5mx6vnk0yfvtjvv50wy0qqqqqyrlp7 | Elrond |
| EVM-Compatible Address | **evm** | 20-byte hexadecimal address, starts with 0x | 0x32Be343B94f860124dC4fEe278FDCBD38C102D88 | Ethereum, Binance Smart Chain, Avalanche C-Chain, Polygon, Fantom, Cronos, Arbitrum, Optimism, Celo (1 of 2), Shiba Inu, Chainlink, Uniswap, LEO, Axie Infinity, Aave, Immutable X, etc. |
| Filecoin Address | **filecoin** | Starts with f, followed by numeric identifier | f1g6f4mnzp3y5vztlbdpou6boqiul7ehq6k6cq5mi | Filecoin |
| Flow Address | **flow** | Hexadecimal, starts with 0x | 0x1654653399040a61 | Flow |
| Harmony (ONE Address) | **harmony** | Bech32 encoding, starts with one | one1wy9psm6zkcy7fmyfnkaawxu7yvl9rrkyfsh70p | Harmony |
| Hedera Hashgraph (HBAR Address) | **hedera** | Numeric, in the shard.realm.account format | 0.0.12345 | Hedera Hashgraph |
| IoTeX Address | **iotex** | Bech32 encoding, starts with io | io1xft7mrcqrp3tll40tyevy2xlcn3txprqnywsag | IoTeX |
| Kava Address | **kava** | Bech32 encoding, starts with kava | kava1r5jqyycyaywkgj9d8tczq6wsj97tjt32lhg5e2 | Kava |
| Litecoin Address | **litecoin** | Base58Check encoding, starts with L or M | LZThpCSANBzC6VeSBJP4mtWmXdrrSZ3MJd | Litecoin |
| Near Protocol (Human-Readable) | **near-readable** | Human-readable, alphanumeric string | example.near | Near Protocol |
| Near Protocol (Implicit) | **near-implicit** | 32-byte hexadecimal address, starts with ed25519: | ed25519:3fR5fA7dFpKdC7e6e9eG32fJ6aP7rH89ty1Z9h67z8bB | Near Protocol |
| NEO Address | **neo** | Base58Check encoding, starts with A | ANvHXiWLmA8YZd9FkmHQeHtZ9gdZbhxS5t | NEO |
| Polkadot / Kusama (SS58 Address) | **polkadot** | SS58 encoding, variable length | 15xtDpbALSMgQBU3F45jNSctP2CpL6B57aPtPxw3TGCRxFmA | Polkadot, Kusama |
| Quant Address | **quant** | EVM-compatible, starts with 0x | 0x4a220e6096b25eadb88358cb44068a3248254675 | Quant |
| Ripple (XRP Ledger Address) | **ripple** | Base58Check encoding, starts with r | rDsbeomae4FXwgQTJp9Rs64Qg9vDiTCdBv | XRP Ledger |
| Solana Address | **solana** | Base58, 32-byte public key | H3UQGgbFg1ZN49GjcpGrg3yJc1bE7X2tszXBJD3oYEkj | Solana |
| Stacks Address | **stacks** | Base58Check encoding, starts with SP | SP2J98B2X03X7C0XZECM64K30BJJYQHWBQ6N4NW | Stacks |
| Stellar Address | **stellar** | Base32 encoding, starts with G | GDRXE2BQUC3AZ6JLEO2AB6I75E6S3JXBM5MQCM6PZES5SOZXJ5KL5S7A | Stellar |
| Tezos Address | **tezos** | Base58 encoding, starts with tz | tz1VSUr8wwNhLAzempoch5d6hLRiTh8Cjcjb | Tezos |
| Theta Address | **theta** | Hexadecimal, starts with 0x | 0x1234567890abcdef1234567890abcdef12345678 | Theta |
| Thorchain Address | **thorchain** | Bech32 encoding, starts with thor | thor1q8gdvhg42pxgj8e6qrcqr8grxwqdmkggxqdsjj | Thorchain |
| Toncoin Address | **ton** | Base64 or hexadecimal | Ef8AAPpR-Gd7y4stMUiG9rwOYSHXZGpA9tT9E8xodIXE2E8Z | Toncoin |
| Tron Address | **tron** | Base58Check encoding, starts with T | TJRyLoLgy9wwA14AGr3Zq34ZmCLhi8VGhw | Tron |
| VeChain Address | **vechain** | Hexadecimal, starts with 0x | 0xAa22Bd4aCA19C1E9F8Ea8b71B9136BB6A2D8bF34 | VeChain |


## Are Blockhain Adderss Suitable for CUBID
Yes, in general, very much so. Similar to how we send one time passcode to a phone: assuming that it goes to a smartphone with biometric protection, this would ensure that only the owner can use that phone number, can read a one-time-passcode (OTP) sent to the phone, and can repeat the OTP for verification. Each public key / blockchain account has a "built-in" private key that only the owner knows. We can ask the owner to sign a transaction prooving that they know the private key, which satisfies our requirements for identification. This way users can identify themselves as owners of an account.

But just like any other identifier, blockchain accounts can also be compromized, for example through mistakes, hacking or extorsion. CUBID assumes that all "stamps" are OK to use as identifiers until they are proven Not OK, at which point we immediately blacklist them. 

## Are All Chains Suitable for CUBID?
No. Only chains where reuse of an address is encouraged or the de facto standard would be suitable as personal identifiers within CUBID.

This section categorizes blockchain address types based on their suitability as individual identifiers. It distinguishes between chains that encourage address reuse, those where reuse is possible but privacy concerns exist, and those that primarily promote single-use addresses, making them less suitable for persistent identification.

### 1) Suitable as Individual Identifiers
These chains typically promote or support address reuse, making them suitable as persistent individual identifiers:

- Algorand Address (algorand)
- Aptos Address (aptos)
- Avalanche X-Chain Address (avalanche-x)
- Cardano (Shelley Address) (cardano)
- Celo (Mobile Addressing) (celo-mobile)
- Chiliz Address (chiliz)
- Cosmos / Terra Address (cosmos-terra)
- Elrond (eGLD Address) (elrond)
- EVM-Compatible Address (evm)
- Filecoin Address (filecoin)
- Flow Address (flow)
- Harmony (ONE Address) (harmony)
- Hedera Hashgraph (HBAR Address) (hedera)
- IoTeX Address (iotex)
- Kava Address (kava)
- Litecoin Address (litecoin)
- Near Protocol (Human-Readable) (near-readable)
- NEO Address (neo)
- Polkadot / Kusama (SS58 Address) (polkadot)
- Quant Address (quant)
- Ripple (XRP Ledger Address) (ripple)
- Solana Address (solana)
- Stacks Address (stacks)
- Stellar Address (stellar)
- Tezos Address (tezos)
- Theta Address (theta)
- Tron Address (tron)
- VeChain Address (vechain)

### 2) Questionable as Individual Identifiers
These chains might support reuse in some applications but also promote privacy, where one-time-use addresses could be encouraged in other applications. CUBID Supports these, but we encourage developers to consider the usability of these as individual identifiers.

- Bitcoin (Bech32 / SegWit) (bitcoin-segwit)
- Bitcoin (P2PKH) (bitcoin-p2pkh)
- Bitcoin (P2SH) (bitcoin-p2sh)
- Near Protocol (Implicit) (near-implicit)
- Thorchain Address (thorchain)
- Toncoin Address (ton)

### 3) Not Suitable as Individual Identifiers
These chains focus on privacy and encourage one-time-use addresses, making them unsuitable for individual identifiers. CUBID does not support these chains for that reason.

- Holo Address
- Internet Computer (ICP Address)
- Monero Address
- Zcash (Shielded Address)

## Identifiers by chain / protocol
Here’s an **alphabetically sorted** list of the **top 50 chains and protocols** at time of writing this, with their **applicable identifiers**, and with cross-chain protocols noted in brackets:

- **Algorand**: algorand  
- **Aptos**: aptos  
- **Arbitrum**: evm  
- **Avalanche**: evm, avalanche-x  
- **Aave**: evm  
- **Axie Infinity**: evm  
- **Bitcoin**: bitcoin-p2pkh, bitcoin-p2sh, bitcoin-segwit  
- **Bitcoin Cash**: bitcoin-p2pkh, bitcoin-p2sh, bitcoin-segwit  
- **BNB (Binance Coin)**: evm  
- **Cardano**: cardano  
- **Chainlink**: evm  
- **Chiliz**: chiliz  
- **Cosmos**: cosmos-terra  
- **Cronos**: evm  
- **Dai**: evm (cross-chain protocol)  
- **Decentraland**: evm  
- **Dogecoin**: bitcoin-p2pkh  
- **Elrond (MultiversX)**: elrond  
- **Ethereum**: evm  
- **Ethereum Classic**: evm  
- **Fantom**: evm  
- **Filecoin**: filecoin  
- **Flow**: flow  
- **Gnosis Chain**: evm  
- **Hedera**: hedera  
- **Immutable X**: evm  
- *Internet Computer*: (not supported)  
- **LEO Token**: evm  
- **Litecoin**: litecoin  
- *Monero*: (not supported)  
- **Near Protocol**: near-readable, near-implicit  
- **Polkadot**: polkadot  
- **Polygon**: evm  
- **Quant**: quant  
- **Render Token**: evm  
- **Ripple (XRP)**: ripple  
- **Shiba Inu**: evm  
- **Solana**: solana  
- **Stacks**: stacks  
- **Stellar**: stellar  
- **Tether**: evm (cross-chain protocol)  
- **The Sandbox**: evm  
- **Thorchain**: thorchain  
- **Toncoin**: ton  
- **TRON**: tron  
- **Uniswap**: evm  
- **USD Coin**: evm (cross-chain protocol)  
- **VeChain**: vechain  
- **Wrapped Bitcoin**: evm (cross-chain protocol)  
- **XRP (Ripple)**: ripple  
- *Zcash*: (not supported)  

### Future Blockchain StampType Support: Roadmap

Cubid’s **blockchain-specific StampTypes** are designed to provide decentralized identity verification using blockchain assets. Currently, our system supports **account ownership verification**, but we plan to expand this capability in the near future to include additional metrics such as **NFT ownership**, **total value held**, **total value locked (TVL)**, and **length of time in the ecosystem**. Below is an overview of both the current functionality and our future plans.

As part of our roadmap, CUBID will expand blockchain StampType capabilities to include more sophisticated forms of identity verification and trust scoring based on various blockchain metrics.

1. **NFT Ownership**
   - **Overview**: This will allow users to prove ownership of **non-fungible tokens (NFTs)**, providing a new way to verify unique assets held by users.
   - **How It Will Work**: The system will query the blockchain to verify that a specific wallet holds one or more NFTs. This will be implemented through smart contracts and validated directly from the user’s wallet.
   - **Use Cases**:
     - **Exclusive Access**: NFTs can be used to unlock special privileges or content in dApps.
     - **Identity Badging**: Users can verify their participation in specific events or ownership of rare digital assets.

2. **Total Value Held**
   - **Overview**: This StampType will allow dApps to verify the total cryptocurrency or token value held in a user’s wallet.
   - **How It Will Work**: By querying the blockchain, the total value of assets held in the user's wallet will be calculated, potentially including major cryptocurrencies and tokens.
   - **Use Cases**:
     - **Wealth Verification**: This can be used for applications where users with higher asset holdings are granted additional privileges or higher trust scores.
     - **Trust Scoring**: Users with significant asset holdings may be perceived as more trustworthy and less likely to engage in malicious behavior.

3. **Total Value Locked (TVL)**
   - **Overview**: This will verify the amount of assets a user has staked or locked in decentralized finance (DeFi) protocols, such as liquidity pools or staking contracts.
   - **How It Will Work**: The system will query DeFi contracts to retrieve the amount of value a user has committed, either as liquidity, collateral, or staked assets.
   - **Use Cases**:
     - **Reputation Building**: Users with high TVL can gain higher trust scores, as they demonstrate a vested interest in the ecosystem’s health.
     - **Access to DeFi Services**: Higher TVL can be a requirement for accessing certain DeFi-based features or rewards within dApps.

4. **Length of Time in Ecosystem**
   - **Overview**: This StampType will track how long a user’s wallet has been active in the blockchain ecosystem, serving as a measure of longevity and consistency.
   - **How It Will Work**: The system will calculate the length of time a wallet has existed based on its first transaction or the earliest recorded activity.
   - **Use Cases**:
     - **Trust Scoring**: Long-standing wallets are more likely to belong to legitimate users, and they can be awarded higher trust scores.
     - **Eligibility for Long-Term Rewards**: dApps can use this metric to provide loyalty-based rewards or privileges to users who have been in the ecosystem for a longer period.

#### Benefits of Future Blockchain StampTypes
- **Enhanced Trust**: By expanding beyond basic account ownership, these new StampTypes will allow for more nuanced trust scores, based on real, verifiable data from the blockchain.
- **Improved Sybil Resistance**: Metrics such as TVL and length of time in the ecosystem make it harder for attackers to generate multiple fake accounts, as these require sustained investment and participation.
- **Broader dApp Use Cases**: dApps will have access to a richer set of data to personalize user experiences, reward users, and implement secure decentralized governance mechanisms.

By introducing these new blockchain metrics, CUBID will enable dApps to gain a more comprehensive view of user identity and trustworthiness, while continuing to ensure privacy and user control over their data.