# Files

## Identities file

Stores all `email identities` the local user has created.   
The file uses the `Java Properties` format.

The following property keys are currently stored and recognized (N is an integer index):

| Field                | Description                                                                     |
|----------------------|---------------------------------------------------------------------------------|
|identityN.publicName  | The public name of the identity, included in emails                             |
|identityN.key         | `Bote Identity`                                                                 |
|identityN.salt        | Salt used to generate a fingerprint                                             |
|identityN.description | Description of the `Bote Identity`, only displayed locally                      |
|identityN.picture     | User picture in a browser-compatible format (JPG, PNG, GIF) encoded with Base64 |
|identityN.text        | Text associated with the `Bote Identity`                                        |
|identityN.published   | Has the `Bote Identity` been published to the public addressbook?               |

You can find information about `Bote Identity` in [Cryptography](https://bote.readthedocs.io/en/latest/v5/cryptography#2-email-identity-formats) of I2P/Bote protocol.

The identities file can optionally contain the property key `default`, with its value set to an `Email Destination`.

## Address Book

See [Address Book](addressbook.md).

## Password Encryption

The identities file, the address book, and all email files are encrypted with AES-256.   
To generate the AES key from the password, scrypt (http://www.tarsnap.com/scrypt.html) is used.   
The file format is:

| Field | #bytes | Description                                   |
|-------|--------|-----------------------------------------------|
| SOF   | 4      | Start of file, contains the characters "IBef" |
| VER   | 1      | Format version, must be 1                     |
| N     | 4      | scrypt CPU cost parameter                     |
| r     | 4      | scrypt memory cost parameter                  |
| p     | 4      | scrypt parallelization parameter              |
| SALT  | 32     | Salt for scrypt                               |
| IV    | 32     | IV for AES                                    |
| DATA  |        | The encrypted data                            |

Parameters N through SALT are cached in a file named derivparams so all encrypted files can use the same key derivation parameters.   
This makes decryption much faster because the key only needs to be derived once per session.
