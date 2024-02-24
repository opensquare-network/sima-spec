# SIMA Delegation

As the delegation section of SIMA spec, this spec describes a way to help delegators publish their announcements to
show their authority, vote policy, or anything else they want to attract more delegates.

## Description object

Delegators' description info will be described in a JSON object. Fields include:

- shortDescription: A 180 character preview description.
- longDescription: A fully customizable Markdown format description of you or your organization.
- isOrganization: Boolean indicating whether the candidate is an organization.

## On-chain remark

Users will submit a `system#remark` extrinsic to publish the delegation announcement JSON object, and the remark format
is `SIMA:D:[VERSION]:S:[DESCRIPTION_OBJECT_CID]`.

- SIMA: it indicates SIMA spec.
- D: it indicates the delegation subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA delegation. The value will be different if a user uses a
  different version.
- S: it means submission, and a description json object cid will be followed.
- DESCRIPTION_OBJECT_CID: it indicates the delegation description JSON object CID.

## Unset

Users will submit a `system#remark` extrinsic to cancel the delegation announcement, and the remark format
is `SIMA:D:[VERSION]:U`.

- SIMA: it indicates SIMA spec.
- D: it indicates the delegation subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA delegation. The value will be different if a user uses a
  different version.
- U: it means unset current delegation announcement. This directive will take no effect if the extrinsic caller didn't
  publish any announcements before.

## Notes

- If an account has proxies, one of the proxy accounts can provide submissions on behalf of the account with
  a `proxy#proxy` call.
- If the remark call is wrapped in a multisig call, the delegation announcements will be set to the multisig account.
- If the resource identified by the `DESCRIPTION_OBJECT_CID` is not a valid JSON object or doesn't contain required
  fields, the delegation announcement settings will be invalid.
- A new delegation announcement will take place of the old one.
