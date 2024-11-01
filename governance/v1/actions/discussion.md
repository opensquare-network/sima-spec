### Start a discussion

Any community member with a polkadot key can start a discussion. An example of the action data to be signed is shown as
follows.

```jsonld=
{
  "action": "new_discussion",
  "title": "discussion title",
  "content": "discussion content",
  "content_format": "subsquare_md",
  "timestamp": 1680592723149
}
```

- action: every action should have this field. It indicates the action type.
- title: title of the discussion. It should not be empty, and spec implemeters should reject data with empty title.
- content: content of the discussion, should be decoded by the way indicated by `content_format` field.
- content_format: it indicates the format of discussion content. At the time of writting, the formats
  contains `subsquare_md` and `HTML`. `subsquare_md` means subsquare supported markdown. `HTML` means
  just [HTML](https://en.wikipedia.org/wiki/HTML).
- timestamp: time of discussion created time. Spec implementers should not accept a user submit a discussion with a
  timestamp much earlier or later than current time.

Data should be signed before submitting to spec implementers. The signed data which will be submitted is as follows.

```jsonld=
{
  "entity": {
    "action": "new_discussion",
    "title": "discussion title",
    "content": "discussion content",
    "content_format": "subsquare_md",
    "timestamp": 1680592723149
  },
  "address": "15ifSDJD2wA7XWwDsitFCHu3wsEfkeBESSxkQg3q8sHqAF2R",
  "signature": "0xa8d02f1744f9ed551fc62e9f41ae2e997c86642a96cad818dca666353a8e4e22ec7c6b6369167d050b58840a12c34a06324e14163705605a388d976acee58384"
}
```

IPFS CID of the signed data is `bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle`. A discussion can be
identified by this CID.

### Append a discussion

A discussion author may need to amend the content due to typos, wrong calculations, outdated numbers, etc. But amending
the content directly will change the IPFS CID, so we will support append content to a discussion instead of amending the
legacy content.

```jsonld=
{
  "action": "append_discussion",
  "CID": "bafybeicx6lf2ppu7qealce55maanaefwdiej3ogms2j5yivllz2p7iujle",
  "content": "appended discussion content",
  "content_format": "subsquare_md",
  "timestamp": 1680598637950
}
```

CID field value should be the CID of the target discussion data CID. Other fields keeps same meaning with those of the
action 'start a discussion'. The whole action data should also include `address` and `signature` fields.

Note:

- The address should be same with that of the `start a discussion` action.
