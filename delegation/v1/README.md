# SIMA Delegation

As the delegation section of SIMA spec, this spec describes a way to help delegators publish their announcements to
show their authority, vote policy, or anything else they want to attract more delegations.

A standard JSON based object is required to describe a delegate's announcement. A delegate can publish his/her
announcement by submitting the CID of the corresponding announcement object to blockchain, or signing the announcement
object and submit to a 3rd party which will submit the announcement to blockchain on behalf of him/her.

## Delegation announcement object

A delegate's announcement info will be described in a JSON object whose fields include:

- shortDescription: A 180 character preview description.
- longDescription: A fully customizable Markdown format description of you or your organization.
- isOrganization: Boolean indicating whether the candidate is an organization.
- timestamp: Time of this announcement.

An example can be:

```json
{
  "shortDescription": "please delegate to me",
  "longDescription": "please delegate to me",
  "isOrganization": false,
  "timestamp": 1712751241940
}
```

Its IPFS CID is `bafybeiaulaj2bn7m5iafyyljbaals4i5aqppv4whl73bgdirfoptkw3veu`.

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

Using upper section example, the remark text should be
`SIMA:D:1:S:bafybeiaulaj2bn7m5iafyyljbaals4i5aqppv4whl73bgdirfoptkw3veu`.

## Submission by an agent

An agency, usually a spec implementer, can help users to submit announcements to IPFS and blockchain. It will request a
JSON object which contains both the delegation announcement object and the corresponding user's signature which we call
it signed description object. The JSON object format will be:

- entity
    - shortDescription
    - longDescription
    - isOrganization
    - timestamp
- address
- signature

The remark format by agencies is a little different: `SIMA:D:[VERSION]:AS:[SIGNED_DESCRIPTION_OBJECT_CID]`. There are
mainly 2 differences:

1. The command is `AS` instead of `S` which represents `agent submission`.
2. The last parameter is the CID of a **signed** description object instead of a raw description object.

Using upper example, the JSON object will be

```json
{
  "entity": {
    "shortDescription": "please delegate to me",
    "longDescription": "please delegate to me",
    "isOrganization": false,
    "timestamp": 1712751241940
  },
  "address": "15ifSDJD2wA7XWwDsitFCHu3wsEfkeBESSxkQg3q8sHqAF2R",
  "signature": "0x9ee721d19e6dac03f04228466f5f60541cd3ee64d20ce22f613b301922dac650cf1fb4aee152856d221d9a81c74ff68a11b709a8551727a20764ce44cd830787"
}
```

IPFS CID of this object is `bafybeig4z2mg3kds6d522a3ztutcqhszcxbyf4hs26ylmqyspbqogev5e4`. Then the remark by an agent is
`SIMA:D:1:AS:bafybeig4z2mg3kds6d522a3ztutcqhszcxbyf4hs26ylmqyspbqogev5e4`.

## Unset

Users will submit a `system#remark` extrinsic to cancel the delegation announcement, and the remark format
is `SIMA:D:[VERSION]:U`.

- SIMA: it indicates SIMA spec.
- D: it indicates the delegation subsection in SIMA spec.
- [version]: 1 for example. it indicates the version of SIMA delegation. The value will be different if a user uses a
  different version.
- U: it means unset current delegation announcement. This directive will take no effect if the extrinsic caller didn't
  publish any announcements before.

## Unset by an agent

An agent can unset a delegate's announcement by submitting a `system#remark` extrinsic, and the remark format should
be `SIMA:D:[VERSION]:AU:[SIGNED_UNSET_DELEGATION_OBJECT_CID]`.

- `SIMA`, `D` and `[VERSION]` have same meanings described in upper sections.
- AU: it means `agent unset`, an unset action by an agency.
- [SIGNED_UNSET_DELEGATION_OBJECT_CID]: it means the CID of a **signed** unset delegation object.

Fields of unset delegation announcement object is as follows:

- action: it's a fixed string: `unset-delegation-announcement`.
- timestamp: time of this action.

An example is:

```json
{
  "action": "unset-delegation-announcement",
  "timestamp": 1712755738457
}
```

The signed object example is

```json
{
  "entity": {
    "action": "unset-delegation-announcement",
    "timestamp": 1712755738457
  },
  "address": "15ifSDJD2wA7XWwDsitFCHu3wsEfkeBESSxkQg3q8sHqAF2R",
  "signature": "0x320947f498aee04d69805e66d83f9e41211fc52c93621d564ce4cca5af382c3fe041900484bbe593af0d896f83b3e3ff1baddf398a94f2e32d67c390b1e4a980"
}
```

CID of upper signed object is `bafybeidvlxpgat4ly4wolrhuwttsqjuaxlfvw4tckbvdz35z4wd7lruroa`. So the remark text by an
agent is `SIMA:D:1:AU:bafybeidvlxpgat4ly4wolrhuwttsqjuaxlfvw4tckbvdz35z4wd7lruroa`.

## Batch submission by an agent

To save gas consumption, an agent can submit a CID which represent a batch of signed description objects with a
pre-defined remark format `SIMA:D:[VERSION]:BAS:[CID_OF_MULTIPLE_SIGNED_DESCRIPTION_OBJECT_CID]`.

1. The command is `BAS` which represents `batch agent submission`.
2. The last parameter is the CID of a JSON array which contains CIDs of a group of signed description objects.

```jsonld=
[
  "bafybeig4z2mg3kds6d522a3ztutcqhszcxbyf4hs26ylmqyspbqogev5e4",
]
```

## Notes

- If an account has proxies, one of the proxy accounts can provide submissions on behalf of the account with
  a `proxy#proxy` call.
- If the remark call is wrapped in a multisig call, the delegation announcements will be set to the multisig account.
- If the resource identified by the `DESCRIPTION_OBJECT_CID` is not a valid JSON object or doesn't contain required
  fields, the delegation announcement settings will be invalid.
- A new delegation announcement will take place of the old one.
