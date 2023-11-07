> Public schemes are good for initial key handling and signatures, but Symmetric crypto and MAC preferred for its speed and efficiency

# Key Establishment
> Like hybrid-encryption, setup a 'throwaway' key for a session.
- Symmetric, random, fresh
- session-lived, for back and forth messages
- between two parties, needs to be authenticated
1. Key pre-distribution (Without/before crypto)
2. Key distribution (use long-term credentials to create symmetric key)
3. Key agreement (public-crypto using certs and signatures)
## Key Predistribution
Someone (a Trusted Authority) gives everyone symmetric keys to use
**Advantages**
- Only need TA before any messages take place
- No need for 'expensive' Asymmetric crypto
- Low communication overhead
**Disadvantages**
- Cumbersome initial secure delivery
- Large secure storage required for each user
- Low flexibility (how do you add new parties?)
- Long-lived keys
> The TA can by definition decrypt and encrypt messages. It knows all

