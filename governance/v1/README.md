# SIMA Governance

As governance section of SIMA specification, this spec decentralize the off-chain discussion data of the community.

## Problem statement

There are several onchain items to be governed, including onchain treasury, parameters, runtime on the polkadot/kusama
blockchain. They can be adjusted by specific permissioned calls. Usually a proposer propose one or a batch of calls
which will be voted by all token holders or a group of authorized members.

Proposers usually start a discussion by creating a post on forums like polkassembly, subsquare and polkadot forum before
submitting the proposal on chain. They leave related context after proposals submitted. Community members can leave
comments and have a discussion during the whole process of governance proposals. These forum platforms usually have
public apis which can be called to get the context info, so community members can save them and platforms can call each
other to sync the data, but there are still some problems with current model.

1. We can not verify the users' data. This is a common problem in web2 environment. Maybe these platforms have no
   motivations to tamper with the data, but we need more verification, less trust. So we can be 100% sure data belongs
   to authors.
2. Users' data maybe lost with a platform's misoperation, or a platform may just stop operations. We need a way to keep
   the data always accessible.
3. Data should be more auditable. Currently, platforms don't save all the data modifications. User can modify the legacy
   leaved context, and the website will just show the context is updated, so users may not be able to see the old data
   which may include the proposal owner's commitments.
4. The more one single centralized governance platform own users' data, the less possibility another platform can
   provide better solutions. We need a relatively decentralize data hosting solution which will not rely on single
   platforms or a dedicated group of people.
5. Differences of different platforms' API data format make it hard to sync and adapt all other platforms' data.

All in all, we need a more verifiable, auditable, always accessible, decentralized way to host users' off-chain
governance data.

## User roles

Users will be classified into following 3 roles in this section. Proposal proposer and community members will have
discussions based on various governance proposals, they signed their discussion data in a pre-defined format, submit to
spec implementers, then spec implementers will upload the data proof to blockchains.

- Proposal proposer: this role can provide context for proposals he/she proposed.
- Community member: this role need a polkadot key to participate in discussions.
- Spec implementer: teams like polkassembly, subsquare, nova wallet maybe the potential implementers.
  They accept other roles signed data, host them and upload the data IPFS CID to the corresponding blockchain.

## Proposal indexer

We use a json object which contains specified lower cased fields to identify a specific onchain proposal.

- pallet: it will be the pallet name the proposal is hosted on-chain.
- object: it will be named after the proposal storage type in a specified pallet, for example `proposals` in `treasury`
  pallet.
- proposed_height: it is the blockchain height at which the object is proposed.
- id: the identifier of the object. It maybe proposal index, hash, or other types that can locate the object.
- ids: it's array type, in case of the object is stored in a double or multiple map that we need multiple identifiers.

`id` and `ids` are mutually exclusive. `id` will be used in most cases, while `ids` will be used only when the object
has multiple identifiers and `id` is not enough to locate the object.

A OpenGov referenda example can be:

```jsonld=
{
  "pallet": "referenda",
  "object": "referendumInfoFor",
  "proposed_height": 16741061,
  "id": 100
}
```

A treasury proposal example can be:

```jsonld=
{
  "pallet": "treasury",
  "object": "proposals",
  "proposed_height": 9438552,
  "id": 100
}
```

A council motion proposal example can be:

```jsonld=
{
  "pallet": "council",
  "object": "proposalOf",
  "proposed_height": 6207756,
  "id": "0x7d6b3f8d6d91c00460a2f53477d308f5b47c500a667adecedfee4fd869472643"
}
```

## User actions

There are 2 kinds of posts in a governance platform. One type is discussion post while another is proposal post.
Different types of posts will have different types of actions. Every action will have a unique id which will be used in
the signed data.

For discussion posts, user actions are shown in following table.

| Action              | Id                |
|---------------------|-------------------|
| Start a discussion  | new_discussion    |
| Append a discussion | append_discussion |
| Comment             | comment           |
| Replace a comment   | replace_comment   |
| Upvote              | upvote            |
| Downvote            | downvote          |
| Cancel upvote       | cancel_upvote     |
| Cancel downvote     | cancel_downvote   |

For proposal posts, user actions are shown in following table.

| Action              | Id                |
|---------------------|-------------------|
| Provide context     | provide_context   |
| Comment a proposal  | comment_proposal  |
| Upvote a proposal   | upvote_proposal   |
| Downvote a proposal | downvote_proposal |
| Comment             | comment           |
| Replace a comment   | replace_comment   |
| Upvote              | upvote            |
| Downvote            | downvote          |
| Cancel upvote       | cancel_upvote     |
| Cancel downvote     | cancel_downvote   |

### Action object

Action object is a JSON object describing a user's action. An action object will contain 3 fields.

- entity: it describes the action type and content. Different action entities will have different fields.
- address: it indicates the action author who will sign the action entity.
- signature: signature of the address to the entity object. Signature should be calculated
  by `polkadot_key_sign(JSON.stringify(entity))`.

We will describe entities for various actions in following sections.

### Authority

An account can perform actions on behalf of another account. An extra field `real` will be added to indicate these
cases. Value of `real` field will be a JSON object which contains 2 fields:

- address: this indicates the real address who is on behalf of.
- section: this value maybe `multisig` or `proxy`.

#### Multisig

If it's a multisig account who propose an on-chain proposal, anyone of the signatories can provide context for the
proposal. The `real` value of an action object will contain fields:

- address: the multisig address who is on behalf of.
- section: `multisig`.

#### Proxy

If an account has proxies, one of the proxy accounts can provide actions on behalf of the account. The extra `real`
value of an action object will contain fields:

- address: the proxied account who is on behalf of.
- section: `proxy`.

Terminology:

- If A set a proxy to B, then B is the proxy(delegate) account, and A is the proxied account.

### Discussion specific actions

Discussion specific actions include `new_discussion` and `append_discussion`. Please check details of these
actions [here](./actions/discussion.md).

### Proposal specific actions

Discussion specific actions include `provide_context`, `comment_proposal`, `upvote_proposal` and `downvote_proposal`.
Please check details of these actions [here](./actions/proposal.md).

#### Common actions

Common actions include `comment`, `replace_comment`, `upvote`, `downvote`, `cancel_upvote` and `cancel_downvote`. Please
check details of these actions [here](./actions/proposal.md).

## Decentralization

SIMA doesn't require a peer to peer network to decentralize users' data. It relies on the specified blockchain's
decentralization and IPFS decentralization mechanism.

- Users' actions data is hosted by IPFS nodes. These nodes can be hosted by spec implementers or public ones
  like [infura](https://infura.io/), [cloudflare](https://www.cloudflare.com/web3/), etc. The data should be finally
  saved to decentralized solutions like [arweave](https://arweave.org/), [crust](https://crust.network/) or other
  solutions by spec implementers.
- Spec implementers will submit the user data proofs to blockchain in `system#remark` extrinsics.

### Data proof submission

There will be different spec implementers. Each of these implementers accepts users' signed action data and save them to
servers at the first step. Spec implementers are responsible to do following 2 steps work to keep the data decentralized
and public accessible.

1. Upload the received users' signed actions data to one or more public IPFS nodes. Spec implementers should keep these
   data always accessible. They can pin them at a specified frequency, or they can submit these data to decentralized
   storage solutions like arweave, crust, etc.
2. Submit IPFS CIDs of these data to blockchain in a conventional way.

In the 2nd step, spec implementers will submit IPFS CID of action data CID array to blockchain. An example is shown as
follows.

```jsonld=
[
  "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle",
  "bafybeid7ktyoum7lzugefurchc4hxveqbohtus5lkibanxjiaodgmlqhve"
]
```

Above is an array of CIDs of user signed action data. Its CID
is `bafybeihylrppf3ufj7dbgdlfyd4vh4dlwnvqlizkjimetxiymhollsf2ka`. Spec implementer will submit this CID to blockchain
with `system#remark` extrinsic. The argument should be:

`0x{bytes(SIMA:G:1:S:{CID_of_user_signed_action_data_CID_array})}`

In the above example, it should be `0x{bytes(SIMA:G:1:S:bafybeihylrppf3ufj7dbgdlfyd4vh4dlwnvqlizkjimetxiymhollsf2ka)}`,
and its final value is
`0x53494d413a473a313a533a6261667962656968796c727070663375666a37646267646c66796434766834646c776e76716c697a6b6a696d65747869796d686f6c6c7366326b61`.

As we can see, action data submission remark follows predefined rules in which several fields are separated by
character `:`. Meaning of each field is described as follows.

- SIMA: it indicates SIMA spec.
- G: it indicates governance subsection in SIMA spec.
- [version]: 1 in the example. it indicates the version of SIMA governance. The value will be different if a user uses a
  different version.
- S: a directive of SIMA governance. Currently, we only has one command in SIMA spec, new commands maybe introduced in
  future versions.
- [CID field]: it should be the CID of user submission data. User submission data can be:
    - an action object.
    - an array of CIDs of action objects.

### Data recovery

Any new SIMA spec implementers can recover users' data in following steps.

1. Scan the target blockchain and filter `system#remark` extrinsics for data proof submission.
2. Extract CIDs from extrinsics.
3. Fetch users action data from public IPFS gateways.
4. Extract user data, do normalization and save to their own databases.

## Constraints

1. Every user has to own a polkadot key to sign actions data. It's not very friendly to web2 users who are used to
   register an account with emails, login into a website, and then do most actions without verification anymore.
2. Users have to sign every action.

## Conclusion

SIMA spec defines user actions which will support users of substrate based blockchain to complete off-chain governance
discussion processes. Every action has predefined format. Users sign the action data, submit them to spec implementers,
and spec implementers will submit the data proof to with a `system#remark` extrinsic and keep the data always
accessible.

Though it has some constraints, we believe it's in the right direction to work in a web3 environment.
