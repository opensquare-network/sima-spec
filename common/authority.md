# Authority

An account can perform actions on behalf of another account. An extra field `real` will be added to various action
objects to indicate these cases. Value of `real` field will be a JSON object which contains 2 fields:

- address: this indicates the real address who is on behalf of.
- section: this value maybe `multisig` or `proxy`.

## Multisig

If it's a multisig account who propose an on-chain proposal, anyone of the signatories can provide context for the
proposal. The `real` value of an action object will contain fields:

- address: the multisig address who is on behalf of.
- section: `multisig`.

## Proxy

If an account has proxies, one of the proxy accounts can provide actions on behalf of the account. The extra `real`
value of an action object will contain fields:

- address: the proxied account who is on behalf of.
- section: `proxy`.

Terminology:

- If A set a proxy to B, then B is the proxy(delegate) account, and A is the proxied account.
