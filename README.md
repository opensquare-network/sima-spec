# SIMA Specification

SIMA defines a set of user actions and data standards to decentralize users' off-chain collaboration data for
[substrate](https://github.com/paritytech/substrate) based blockchains. There are subsections in this spec including
governance discussions, avatar settings, delegation announcements, and they can be extended in future versions.

In general, users sign their actions data with their keys, submit the signed data to spec implementers. Spec
implementers will be responsible for submitting the IPFS CID of user actions data to blockchain with `system#remark`
extrinsics.

SIMA is named after [Sima Qian](https://en.wikipedia.org/wiki/Sima_Qian), one of the most famous Chinese historian.

## Text Format

SIMA uses plain text to represent user actions which are categorized into different subsections. A common SIMA text is
`SIMA:[SUB_SECTION]:[VERSION]:[COMMAND]:[ARGS]`.

- SIMA: it indicates SIMA spec. We need this to differentiate from remark text of other specs.
- SUB_SECTION: subsection of SIMA spec. It is an enum value. Currently, it includes `G` for governance, `A` for avatar
  settings, `D` for delegation announcements.
- VERSION: version of SIMA subsections.
- COMMAND: command of SIMA subsections. It depends on definitions of each subsection.
- ARGS: command args of SIMA subsections. They will be different by different commands, but usually it will just be a
  CID of a JSON data in which there are data presenting user actions.

## Governance Discussions

Blockchain governance enables token holders to decide the evolution of a chain. Proposers submit proposals and token
holders will vote based on token balance in normal cases. Usually governance participators will have discussions in
a forum, and leave their comments with a web2 registered account. These discussions data will be saved in the forum host
database which is hard to be verified by third parties and easy to be tampered with. SIMA governance defines a set rules
to help users decentralize the off-chain discussion data. Details can be checked [here](governance/v1/README.md).

## Avatar Settings

Polkadot has an identity pallet with which user can set specified and customized fields, and the community can know the
verification status by judgements from registrars. Though avatar settings can be achieved by customized fields with
identity pallet. It will be troublesome for users to do update because every update will require another registrar's
judgement.

## Delegation Announcements

Delegation is an important way to increase governance participation. To attract more token holders, delegates can make
some announcement or description about their authority or policy to vote, but currently users can only submit this
information to centralized entities.
