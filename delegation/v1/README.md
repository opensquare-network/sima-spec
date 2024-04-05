# SIMA Delegation

As the delegation section of SIMA spec, this spec describes a way to help delegators publish their announcements to
show their authority, vote policy, or anything else they want to attract more delegates.

## Delegation announcement object

Delegators' description info will be described in a JSON object. Fields include:

- shortDescription: A 180 character preview description.
- longDescription: A fully customizable Markdown format description of you or your organization.
- isOrganization: Boolean indicating whether the candidate is an organization.

## User submission

Users can publish their delegations announcements on chain by themselves. What they need to do is:

1. Create a JSON object with fields described in upper section.
2. Submit the JSON object to an IPFS node and get the CID.
3. Submit a `system#remark` extrinsic in a format which contains the CID got in step 2.

The remark format is `SIMA:D:[VERSION]:S:[DESCRIPTION_OBJECT_CID]`.

- SIMA: it indicates SIMA spec.
- D: it indicates the delegation subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA delegation. The value will be different if a user uses a
  different version.
- S: it means submission, and a description json object cid will be followed.
- DESCRIPTION_OBJECT_CID: it indicates the delegation description JSON object CID.

## Submission by agency

An agency, usually a spec implementer, can help users to submit announcements to IPFS and blockchain. It will request a
JSON object which contains both the delegation announcement object and the corresponding user's signature which we call
it
signed description object. The JSON objection format will be:

- entity
    - shortDescription
    - longDescription
    - isOrganization
- address
- signature

The remark format by agencies is a little different: `SIMA:D:[VERSION]:AS:[SIGNED_DESCRIPTION_OBJECT_CID]`. There are
mainly 2 differences:

1. The command is `AS` instead of `S` which represents `agent submission`.
2. The last parameter is the CID of a **signed** description object instead of a raw description object.

## Unset

Users will submit a `system#remark` extrinsic to cancel the delegation announcement, and the remark format
is `SIMA:D:[VERSION]:U`.

- SIMA: it indicates SIMA spec.
- D: it indicates the delegation subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA delegation. The value will be different if a user uses a
  different version.
- U: it means unset current delegation announcement. This directive will take no effect if the extrinsic caller didn't
  publish any announcements before.

## Unset by agency

An agency can unset a delegator's announcement by submitting a `system#remark` extrinsic, and the remark format should
be `SIMA:D:[VERSION]:AU:[SIGNED_UNSET_DELEGATION_OBJECT_CID]`.

- `SIMA`, `D` and `[VERSION]` have same meanings described in upper sections.
- AU: it means `agent unset`, an unset action by an agency.
- [SIGNED_UNSET_DELEGATION_OBJECT_CID]: it means the CID of a **signed** unset delegation object.

A signed unset delegation object has the following fields:

- entity: it should be a string composed of 3 elements. The first one is predefined
  string `UNSET_DELEGATION_ANNOUNCEMENT`, the 2nd one is a predefined separator `:`, and the third one is an IPFS CID
  which represents the latest delegation announcement object.
- address
- signature: it's the signature to the entity of the address.

## Notes

- If an account has proxies, one of the proxy accounts can provide submissions on behalf of the account with
  a `proxy#proxy` call.
- If the remark call is wrapped in a multisig call, the delegation announcements will be set to the multisig account.
- If the resource identified by the `DESCRIPTION_OBJECT_CID` is not a valid JSON object or doesn't contain required
  fields, the delegation announcement settings will be invalid.
- A new delegation announcement will take place of the old one.
