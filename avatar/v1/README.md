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

## Unset avatar

Users will submit a `system#remark` extrinsic to unset an avatar, and the remark format is `SIMA:A:[VERSION]:U`.

- SIMA: it indicates SIMA spec.
- A: it indicates the avatar subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA avatar. The value will be different if a user uses a
  different version.
- U: it means unset current avatar. This command will take no effect if the extrinsic caller don't have an avatar.

### Notes

- If an account has proxies, one of the proxy accounts can provide actions on behalf of the account with a `proxy#proxy`
  call.
- If the avatar remark call is wrapped in a multisig call, the avatar will be set to the multisig account.
- If the resource identified by the `IMG_CID` is not a valid image, the avatar setting will be invalid.
- A new avatar setting will take place of the old avatar.
