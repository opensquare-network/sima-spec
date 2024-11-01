### Provide context

This action provide context information for an on-chain proposal. This action author should be the proposal author. An
example of this action entity data is shown as follows.

```jsonld=
{
  "action": "provide_context",
  "indexer": {
    "pallet": "treasury",
    "object": "proposals",
    "proposed_height": 9438552,
    "id": 100
  },
  "title": "proposal title",
  "content": "proposal content",
  "content_format": "subsquare_md",
  "timestamp": 1680601098763
}
```

- `indexer` field is a [proposal indexer](#proposal-indexer) for locating a specific on-chain proposal. It will follow
  the definition described in the proposal indexer section.

The whole action data should also include `address` and `signature` fields.

Note:

- A new valid action updating same proposal will override the content by previous actions.

### Comment a proposal

This action leaves a comment to an on-chain proposal. An example of this action entity data is shown as follows.

```jsonld=
{
  "action": "comment_proposal",
  "indexer": {
    "pallet": "treasury",
    "object": "proposals",
    "proposed_height": 9438552,
    "id": 100
  },
  "content": "proposal comment",
  "content_format": "subsquare_md",
  "timestamp": 1680615958881
}
```

### Upvote/downvote a proposal

This action's target is same with that of the `comment_proposal` action, and the target is an onchain proposal. An
example entity is shown as follows.

```jsonld=
{
  "action": "upvote_proposal",
  "indexer": {
    "pallet": "treasury",
    "object": "proposals",
    "proposed_height": 9438552,
    "id": 100
  },
  "timestamp": 1680686606947
}
```
