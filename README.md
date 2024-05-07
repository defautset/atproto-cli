# atproto-cli

This is a minimal shell interface to create, read or delete a text record in the ATProto network - especially for the Bluesky app.

## INSTALL

```sh
bin=atproto src=https://raw.githubusercontent.com/defautset/atproto-cli/main/$bin dst=~/.local/bin; \
mkdir -pv $dst && cd $dst && curl -L -o $bin $src && chmod +x $bin && ./$bin
```

And now you can change the configuration file (~/.config/atproto/atprotorc) or take a look at this example file: https://github.com/defautset/atproto-cli/blob/main/atprotorc.example  
or this:

```sh
# atprotorc to be stored in $HOME/.config/atproto

# Identity:
ATP_HANDLE=fautset.de
ATP_PASSWORD=****-****-****-**** #AliceBlue

# PDS/API:
ATP_XRPC=https://userdir.de/xrpc
ATP_COLLECTION=app.bsky.feed.post

```

### Dependencies:
- bash
- curl, jq
- echo, printf, cat, cut, rev

## USAGE

```sh
$ atproto ping
pong with this version: 0.4.12

$ atproto whoami
{"handle":"fautset.de","email":"****@**********"}
```

```sh
# Create:
$ atproto create Hallo Werft- und Hafenarbeiter. Ahoi.

3krvttgot2c24:

{
  "text": "Hallo Werft- und Hafenarbeiter. Ahoi.",
  "createdAt": "2024-05-07T15:00:35Z"
}

# Read:
$ atproto read 3krvttgot2c24 | jq -r .text
Hallo Werft- und Hafenarbeiter. Ahoi.

# Delete:
$ atproto delete 3krvwlbtchs24
```

Example via BlueskyWeb: https://bsky.app/profile/fautset.de/post/3krvttgot2c24  
 or via the API (XRPC): https://userdir.de/xrpc/com.atproto.repo.getRecord?repo=fautset.de&collection=app.bsky.feed.post&rkey=3krvttgot2c24

```sh
$ curl -sS 'https://userdir.de/xrpc/com.atproto.repo.getRecord?repo=fautset.de&collection=app.bsky.feed.post&rkey=3krvttgot2c24' | jq -r .value.text
Hallo Werft- und Hafenarbeiter. Ahoi.

$ curl -sS 'https://userdir.de/xrpc/com.atproto.repo.listRecords?repo=fautset.de&collection=app.bsky.feed.post&limit=1&reverse=true' | jq -r .records[].value.text
Hallo Werft- und Hafenarbeiter. Ahoi.

$ curl -sS 'https://userdir.de/xrpc/com.atproto.repo.listRecords?repo=fautset.de&collection=app.bsky.feed.post&limit=1&reverse=true' | jq .records
[
  {
    "uri": "at://did:plc:6ss...zqc/app.bsky.feed.post/3krvttgot2c24",
    "cid": "...",
    "value": {
      "text": "Hallo Werft- und Hafenarbeiter. Ahoi.",
      "$type": "app.bsky.feed.post",
      "createdAt": "2024-05-07T15:00:35Z"
    }
  }
]
```
 