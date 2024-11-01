### Comment

The comment action leaves a comment to a discussion or another comment. An example of this action entity data is shown
as follows.

```jsonld=
{
  "action": "comment",
  "cid": "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle",
  "content": "dicussion comment content",
  "content_format": "subsquare_md",
  "timestamp": 1680615958881
}
```

- cid: it indicates the target action this comment is responding to. The target action can be a discussion, or another
  comment.

Note:

- SIMA spec don't support 3rd level comments which means we can leave a comment to a proposal or discussion comment, but
  not a comment of another comment.

### Replace a comment

Editing a discussion comment is not supported directly in SIMA spec. This action replaces an existed discussion comment.
An example of this action entity data is shown as follows.

```jsonld=
{
  "action": "replace_comment",
  "cid": "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle",
  "old_comment_cid": "bafybeid7ktyoum7lzugefurchc4hxveqbohtus5lkibanxjiaodgmlqhve",
  "content": "dicussion new comment content",
  "content_format": "subsquare_md",
  "timestamp": 1680615958881
}
```

- cid: it indicates the target action this old comment is responding to. The target action can be a discussion, or
  another comment.
- old_comment_cid: it indicates the CID of old comment to be replaced.

### Upvote/downvote

This action's target is same with that of the comment action, and the target maybe a discussion or another comment. An
example entity is shown as follows.

```jsonld=
{
  "action": "upvote",
  "cid": "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle"
  "timestamp": 1680686606947
}
```

- cid: it indicates the target action this action is responding to. The target action can be a discussion, or a comment.

### Cancel upvote/downvote

This action's target is same with that of upvote/downvote action. An example entity is shown as follows.

```jsonld=
{
  "action": "cancel_upvote",
  "cid": "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle"
  "timestamp": 1681873995303
}
```

- `action` field can be `cancel_upvote` or `cancel_downvote`.
- `cid` means the corresponding upvote/downvote/upvote_proposal/downvote_proposal action cid.

Note:

- If there is no corresponding upvote/downvote/upvote_proposal/downvote_proposal for same target from same address, this
  action won't take any effect.
