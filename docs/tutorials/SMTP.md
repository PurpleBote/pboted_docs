# SMTP

_Tested with [Mozilla Thunderbird](https://www.thunderbird.net/en-US/)_

!!! warning "Warning"

    At the moment, using SMTP is only possible when filling out the [address book](addressbook.md).

To be able to send email through SMTP you need to:

- Fill `[smtp]` section in [configuration file](../user-guide/configuration.md#smtp):

```ini
[smtp]
enabled = true
address = 127.0.0.1
port = 9025
```

- Restart the **pboted** to apply the settings
- After loading, you be able to connect to the specified SMTP port manually or with your mail client.

## How to compose email

### FROM

There are 2 options for how this field might look.

#### 1. classic

In this case, the address consists of three parts, something like this:

`NAME <IDENTITY_NAME@DOMAIN>`

- `NAME` - can be the name of the used Bote identity
- `IDENTITY_NAME` - can be the name of the used Bote identity
- `DOMAIN` -  can be any valid domain

At least one of the two - `NAME` or `IDENTITY_NAME` - must be present in loaded `Bote identities`  
If both (`NAME` and `IDENTITY_NAME`) are present in in loaded Bote identities  but refer to different `Bote identities` - only the `Bote identity` specified for `IDENTITY_NAME` will be used.  
`DOMAIN` will be ignored anyway.

For example:

```
John Doe <johnd@bote.i2p>
```

#### 2. Bote style

In this case, the address consists of two parts:

`IDENTITY_NAME <BOTE_ADDRESS>`

- `IDENTITY_NAME` - should be the name of the used `Bote identity`
- `BOTE_ADDRESS` - should be `Bote address`

If a `Bote Identity` is found for `IDENTITY_NAME`, the `BOTE_ADDRESS` field will be replaced with the value from that identity.

For example:

```
johnd <24noEIMPvV9CEwrSWQtIsTA7balaZ80ZOGRBAzrsBl5nv9xud~k28d9TQIgXmyyCYtHl8PJASAFDeefSc6EJ81>
```

### TO

Field consists of two parts:

`NAME <ALIAS>`

- `NAME` - can be in the address book for replacement
- `ALIAS` - can be in the address book for replacement

One of the two - `NAME` or `ALIAS` - must be present in the address book.  
If both (`NAME` and `ALIAS`) are present in the address book but refer to different `Mail Destinations` - only the `Mail Destination` specified for `ALIAS` will be used.

For example, if you have this record in addressbook:

```
johnd1@bote.i2p;John Doe 1;24noEIMPvV9CEwrSWQtIsTA7balaZ80ZOGRBAzrsBl5nv9xud~k28d9TQIgXmyyCYtHl8PJASAFDeefSc6EJ81
```

You can fill TO like this:

`TO: John Doe <johnd1@bote.i2p>`
