# SIMA Avatar

As the avatar section of SIMA specification, this spec describe a way to help users set a avatar on substrate based
blockchain in a permission less way.

## Problem statement

Though polkadot has an identity pallet and users can do avatar settings by customized fields, users rarely use it
because of identity pallet has some limitations, for example every update will require another registrar's judgement. To
align users' avatar related behaviours on web2 sites, this spec propose a way to remark avatar settings on blockchain.

## Set avatar

Users will submit a `system#remark` extrinsic to set an avatar, and the remark format is `SIMA:A:[VERSION]:S:[IMG_CID]`.

- SIMA: it indicates SIMA spec.
- A: it indicates the avatar subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA avatar. The value will be different if a user uses a
  different version.
- S: it means submission, and an image cid will be followed.
- IMG_CID: it indicates the avatar image IPFS CID.

## Submission by an agent

An agency, usually a spec implementer, can help users to submit avatar settings to IPFS and blockchain. It will request
a JSON object which contain an entity JSON object which describe a user's avatar setting behavior and the corresponding
user's signature. The JSON object format will be:

- entity
    - action
    - CID
    - timestamp
- address
- signature

An example of the entity JSON object can be:

```json
{
  "action": "set-avatar",
  "CID": "bafybeibcruwoc7aagctjem3xzmskc7pbbhfhavst4dexvmw6tr6gtq4fy4",
  "timestamp": 1716275915587
}
```

The signed JSON object example is:

```json
{
  "entity": {
    "action": "set-avatar",
    "CID": "bafybeibcruwoc7aagctjem3xzmskc7pbbhfhavst4dexvmw6tr6gtq4fy4",
    "timestamp": 1716275915587
  },
  "address": "15ifSDJD2wA7XWwDsitFCHu3wsEfkeBESSxkQg3q8sHqAF2R",
  "signature": "0x1c0e28b6cd9826a6bbd6296cf0aa251998fa92eed32d159959d8acbe4bc48c126191e4a64a3a848422a2894fc1f6855a695e84065c1e4bc95debd5193f61b685"
}
```

There is an optional `real` field in entity to help an account to set an avatar on behalf of another account. Please
check details [here](../../common/authority.md).

CID of upper signed object is `bafkreiemydsjywubt62lk7s4w5bzkylmhqkoeyhypdaaqhphxbsmtllfiq`. The remark text by an agent
is `SIMA:A:1:AS:bafybeidvlxpgat4ly4wolrhuwttsqjuaxlfvw4tckbvdz35z4wd7lruroa`.

1. The command is `AS` instead of `S` which represents `agent submission`.
2. The last parameter is the CID of a **signed** description object which contains an avatar setting entity, an address
   and the address' signature to the entity.

## Unset avatar

Users will submit a `system#remark` extrinsic to unset an avatar, and the remark format is `SIMA:A:[VERSION]:U`.

- SIMA: it indicates SIMA spec.
- A: it indicates the avatar subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA avatar. The value will be different if a user uses a
  different version.
- U: it means unset current avatar. This directive will take no effect if the extrinsic caller don't have an avatar.

## Unset by an agent

Same as agency submission of avatar setting, an agency can submit a user's avatar unset behavior to IPFS and blockchain.
It will request a JSON object which contain an entity JSON object which describe a user's avatar unset behavior and the
corresponding user's signature. The JSON object format will be:

- entity
    - action
    - timestamp
- address
- signature

An example of the entity JSON object can be:

```json
{
  "action": "unset-avatar",
  "timestamp": 1716275915587
}
```

There is an optional `real` field to help an account to unset the avatar on behalf of another account. Please check
details [here](../../common/authority.md).

The signed JSON object example is:

```json
{
  "entity": {
    "action": "unset-avatar",
    "timestamp": 1716275915587
  },
  "address": "15ifSDJD2wA7XWwDsitFCHu3wsEfkeBESSxkQg3q8sHqAF2R",
  "signature": "0x2a3ed555d5347d6aad869f02cac98929ebf0e98360eda1525847448d5efb731981de9d5dd6ef465c90cf3402d31f445f1dba8b69284bba60011f23c1363a9e86"
}
```

CID of upper signed object is `bafkreie42easioag2shgdozwgitvey5gjq7wr3jn24xohuwmipdlwyempq`. So the remark text by an
agent is `SIMA:A:1:AU:bafkreie42easioag2shgdozwgitvey5gjq7wr3jn24xohuwmipdlwyempq`.

1. The command is `AU` which means `agent unset`, an avatar unset action by an agency.
2. The last parameter is the CID of a **signed** description object which contains an avatar unset entity, an address
   and the address' signature to the entity.

## Batch submission by an agent

To save gas consumption, an agent can submit a CID which represent a batch of signed description objects with a
pre-defined remark format `SIMA:A:[VERSION]:BAS:[CID_OF_MULTIPLE_SIGNED_DESCRIPTION_OBJECT_CID]`.

An example of JSON with multiple CIDs of avatar related behavior JSON objects is as follows.

```json
[
  "bafybeidvlxpgat4ly4wolrhuwttsqjuaxlfvw4tckbvdz35z4wd7lruroa",
  "bafkreie42easioag2shgdozwgitvey5gjq7wr3jn24xohuwmipdlwyempq"
]
```

### Notes

- If an account has proxies, one of the proxy accounts can provide actions on behalf of the account with a `proxy#proxy`
  call.
- If the avatar remark call is wrapped in a multisig call, the avatar will be set to the multisig account.
- If the resource identified by the `IMG_CID` is not a valid image, the avatar setting will be invalid.
- A new avatar setting will take place of the old avatar.
