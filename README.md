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
Content-Type: application/pgp
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

