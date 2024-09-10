# Introduction to CUBID

CUBID is a decentralized identity management and Sybil attack prevention protocol designed to help apps and users navigate secure identity verification while preserving privacy. It leverages cryptographic techniques, zero-knowledge proofs, and collaboration between apps to ensure that users are real (human) without exposing unnecessary personal data. CUBID's key functionalities include:

1. **Sybil Defense**: It prevents the creation of multiple fake accounts (Sybil attacks) by allowing apps to verify that users are unique without requiring sensitive personal information like government IDs.
   
2. **Reusable Identity**: Users can create a digital identity that can be reused across different apps in the ecosystem, avoiding the need to repeatedly submit verification information.

3. **Selective Disclosure**: Users maintain control over their data, choosing what to share with specific apps. This concept of "identity stamps" lets users verify various facets of their identity while only disclosing relevant information.

4. **APIs for Developers**: CUBID provides APIs that developers can integrate into their apps for secure identity management and Sybil-resistance, enabling quick user onboarding with a low-friction experience.

5. **Collaborative Ecosystem**: CUBID aims to build an ecosystem of apps that collaborate on identity validation and trust-building, strengthening the integrity of the network as more apps participate.

The CUBID identity management platform  enhances security, privacy, and trust across a network of apps and websites, both in Web2 and Web3 (blockchain) environments. We collectively refer to such clients of CUBID as "Apps." Our central thesis is that a shared identity across multiple Apps not only strengthens user trust but also increases security and privacy at the same time.

As users interact with multiple Apps within the CUBID ecosystem, their identity becomes more robust, and their trustworthiness increases. At the same time, the collaborative nature of this ecosystem helps detect and prevent Sybil attacks, as Apps share insights and leverage collective knowledge. To facilitate this collaboration, CUBID uses a combination of Webhooks and APIs to propagate critical updates about identities that are either strengthened or weakened. The key component to making this data interchange possible without disclosing any personal information is the **CUBID Scoring Algorithm**.
- The score of a user represents the likelihood that they are a real user. A higher score means a higher probability that they are a unique and alive human being. Low scores, or even a high but reduced score, signals risk that the user is a bot or an attacker. 
- Users may perform actions that affect their Score across all connected Apps. Inadvertently or intentionally creating multiple CUBID accounts will result in one or more of a user's Stamps being blacklisted. When this happens inadvertently, users must merge their accounts to resolve the blacklisting. Such incidents will temporarily lower their Score but will also restores it immediately afterward. However, for Sybil attackers, the Score remains permanently reduced. 
- In a normal / inadvertent case you should not notice much issues within your App. If however such an attack occurs within your App itself or on the user's auth-account, then you will be notified about the blacklisting event through our Webhooks. This is hopefully then followed by un-blacklisting notification after corrective action has been taken.
- Apps can choose to either use the default scoring algorithm, or make their own.

Beyond managing user identities, CUBID also provides tools for secure Ethereum wallet creation and proof-of-personhood validation. Whether you’re looking to safeguard your platform against bots, manage decentralized identities, or verify user permissions, CUBID offers reliable, scalable solutions tailored to your needs.
