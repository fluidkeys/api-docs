# Keys

## Get an ASCII-armored key by fingerprint

```
GET /key/:fingerprint
```

### Input

| Name        | Type   | Description                                       |
|-------------|--------|----------------------------------------------------
| fingerprint | string | **Required.** Uppercase hex representation of the key's full fingerprint, without spaces.

### Example

```
GET https://api.fluidkeys.com/key/A999B7498D1A8DC473E53C92309F635DAD1B5517
```

### Response

```
Status: 200 Found
Content-Type: text/plain
Content-Length: 1024

-----BEGIN PGP PUBLIC KEY BLOCK-----

...

-----END PGP PUBLIC KEY BLOCK-----
```


## Get an binary-encoded key by fingerprint

```
GET /key/:fingerprint/binary
```

### Input

| Name        | Type   | Description                                       |
|-------------|--------|----------------------------------------------------
| fingerprint | string | **Required.** Uppercase hex representation of the key's full fingerprint, without spaces.

### Example

```
GET https://api.fluidkeys.com/key/A999B7498D1A8DC473E53C92309F635DAD1B5517/binary
```

### Response

```
Status: 200 Found
Content-Type: application/octet-stream
Content-Disposition: attachment; filename="openpgp-public-key-A999B7498D1A8DC473E53C92309F635DAD1B5517.gpg"
Content-Length: 1024

<OpenPGP binary data>
```

# Teams

## Create a team

```
POST /teams
```

### Input

| Name | Type   | Description                                       |
|------|--------|----------------------------------------------------
| name | string | **Required.** A human-readable name for the team. |


### Example

```
POST https://api.fluidkeys.com/teams
Content-Type: application/json
---
{
  "name": "Flextech Ltd"
}
```

### Response

```
Status: 201 Created
Location: https://api.fluidkeys.com/teams/0444db76-dc44-11e8-a0d8-03103dceb1d3
---
{
  "id": "0444db76-dc44-11e8-a0d8-03103dceb1d3",
  "name": "Flextech Ltd"
}

```

# Web Key Directory

Web Key Directory allows clients to discover a public key from an email address.

For example, a client can find a key for 'tina@example.com' by requesting
`https://example.com/.well-known/openpgpkey/hu/<hash>`

## Configure your domain e.g. `example.com`

Redirect `https://example.com/.well-known/openpgpkey/` to `api.fluidkeys.com`:

```
# nginx config
rewrite ^/.well-known/openpgpkey/(.*)$ https://api.fluidkeys.com/wkd/example.com/.well-known/openpgpkey/$1 redirect;
```

## Get OpenPGP public key using Web Key Directory

Note: this endpoint is not intended to be called directly, but by redirecting a domain as shown above.

```
GET /wkd/:domain/.well-known/openpgpkey/hu/:zbase32`
```

### Input

| Name        | Type   | Description                                                                |
|-------------|--------|-----------------------------------------------------------------------------
| domain      | string | **Required.** The mail domain to discover keys for.                        |
| zbase32     | string | **Required.** The Z-Base-32 encoded, SHA1 hashed, lowercase email address. |

### Example

To look up `hello@fluidkeys.com`:

```
GET /wkd/fluidkeys.com/.well-known/openpgpkey/hu/im4cc8qhazwkfsi65a8us1bc5gzk1o4p
```

### Response

```
Status: 302 Found
Location: https://api.fluidkeys.com/key/D923026D0A8DB1C1B418D030EE267A8794D63E30/binary
```


## Get Web Key Directory policy

Returns an empty policy file, indicating that Web Key Directory is supported for this domain.

```
GET /wkd/:domain/.well-known/openpgpkey/policy
```

### Input

| Name        | Type   | Description                                                                  |
|-------------|--------|-------------------------------------------------------------------------------
| domain      | string | **Required.** The mail domain being queries for its Web Key Directory policy |

### Example

```
GET /wkd/example.com/.well-known/openpgpkey/policy
```

### Response

```
Status: 200 Found
Content-Type: text/plain
Content-Length: 0
```
