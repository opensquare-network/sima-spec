# SIMA Specification

SIMA defines a set of indicators, entities and identifiers to decentralize polkadot/kusama governance off-chain discussions.

## Identifiers
There are different types of governance items in the polkadot/kusama governance process, including democracy referenda,
proposals, treasury proposals, bounties, etc. We need an identifier to locate each item for discussions.

- Discussions
  - Post: `P_[IPFS_CID]`.
- Democracy
  - Referendum: `D_R_[started_height]_[referendum_index]`.
  - Proposal: `D_P_[proposed_height]_[proposal_index]`.
  - External Proposal: `D_E_[proposed_height]_[proposal_hash]`.
- Treasury
  - Proposal: `T_P_[proposed_height]_[proposal_index]`.
  - Tip: `T_T_[proposed_height]_[tip_hash]`.
  - Bounty: `T_B_[proposed_height]_[bounty_index]`.
  - Child bounty: `T_B_[parent_bounty_proposed_height]_[parent_bounty_index]_CB_[child_bounty_added_height]_[child_bounty_index].`
- Council
  - Motion: `C_M_[proposed_height]_[motion_hash]`.
- Technical Committee
  - Motion(Proposal): `TC_M_[proposed_height]_[motion_hash]`.
- OpenGov referenda
  - Referendum: `R_R_[submitted_height]_[referendum_index]`.
- OpenGov fellowship
  - Referendum: `F_R_[submitted_height]_[referendum_index]`.
