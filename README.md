# SIMA Specification

SIMA defines a set of user actions and data standards to decentralize users' off-chain collaboration data for
[substrate](https://github.com/paritytech/substrate) based blockchains. User actions maybe related with governance
discussions, avatar settings, delegation announcements, etc.

In general, users sign their actions data with their keys, submit the signed data to spec implementers. Spec
implementers will be responsible for submitting the IPFS CID of user actions data to blockchain with `system#remark`
extrinsics.

SIMA is named after [Sima Qian](https://en.wikipedia.org/wiki/Sima_Qian), one of the most famous Chinese historian.

## Governance Discussions

Blockchain governance enables token holders to decide the evolution of a chain. Proposers submit proposals and token
holders will vote based on token balance in normal cases. Usually governance participators will have discussions in
a forum, and leave their comments with a web2 registered account. These discussions data will be saved in the forum host
database which is hard to be verified by third parties and easy to be tampered with.

## Avatar Settings

Polkadot has an identity pallet with which user can set specified and customized fields, and the community can know the
verification status by judgements from registrars. Though avatar settings can be achieved by customized fields with
identity pallet. It will be troublesome for users to do update because every update will require another registrar's
judgement.

## Delegation Announcements

Delegation is an important way to increase governance participation. To attract more token holders, delegates can make
some announcement or description about their authority or policy to vote, but currently users can only submit this
information to centralized entities.
