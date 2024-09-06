# Introduction to CUBID

CUBID is a versatile identity management platform that enhances security, privacy, and trust across a network of apps and websites, both in Web2 and Web3 (blockchain) environments. We collectively refer to such clients of CUBID as "Apps." Our central thesis is that a shared identity across multiple Apps not only strengthens user trust but also increases security and privacy at the same time.

As users interact with multiple Apps within the CUBID ecosystem, their identity becomes more robust, and their trustworthiness increases. At the same time, the collaborative nature of this ecosystem helps detect and prevent Sybil attacks, as Apps share insights and leverage collective knowledge. To facilitate this collaboration, CUBID uses a combination of Webhooks and APIs to propagate critical updates about identities that are either strengthened or weakened.

Users may perform actions that affect their Score across all connected Apps. Inadvertently or intentionally creating multiple CUBID accounts will result in one or more of a user's Stamps being blacklisted. When this happens inadvertently, users must merge their accounts to resolve the blacklisting. Such incidents will temporarily lower their Score but will also restores it immediately afterward. However, for Sybil attackers, the Score remains permanently reduced. 

In a normal / inadvertent case you should not notice much issues within your App. If however such an attack occurs within your App itself or on the user's auth-account, then you will be notified about the blacklisting event through our Webhooks. This is hopefully then followed by un-blacklisting notification after corrective action has been taken.

Beyond managing user identities, CUBID also provides tools for secure Ethereum wallet creation and proof-of-personhood validation. Whether you’re looking to safeguard your platform against bots, manage decentralized identities, or verify user permissions, CUBID offers reliable, scalable solutions tailored to your needs.
