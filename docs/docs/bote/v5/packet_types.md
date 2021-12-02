# 3. Packet Types

* Version 5 is incompatible to earlier versions.   
* Version 5 clients will not talk to higher versions.     
* Strings in packets are UTF8 encoded.
* `E` denotes encrypted data.   

For supported encryption and signature algorithms, see [7.1 Cryptography](cryptography.md)

## 3.1. Data Packets

All data packets start with one byte for the `Packet type`, followed by one byte for the `Packet version`. 

These are always sent wrapped in a Communication Packet (see 3.2).

### 3.1.1 Email Packet (encrypted)

An email or email fragment, one recipient. Part of the Packet is encrypted with the recipient's key.

| Field   | Data Type  | Description                                                         |
|---------|------------|---------------------------------------------------------------------|
| `TYPE`  | 1 byte     | Value = `'E'`                                                       |
| `VER`   | 1 byte     | Protocol version                                                    |
| `KEY`   | 32 bytes   | DHT key of the Packet, SHA-256 hash of `LEN+DATA`                   |
| `TIM`   | 8 bytes    | The time the Packet was stored on a storage node (int64)  `[VER 5]` |
| `DV`    | 32 bytes   | Delete verification hash, SHA-256 hash of `DA`                      |
| `ALG`   | 1 byte     | ID number of the encryption algorithm used                          |
| `LEN`   | 2 bytes    | Length of `DATA`                                                    |
| `DATA`  | byte[] `E` | Data for decryption and `Unencrypted Email Packet`                  |

ToDo: add info about alg-specific prefixes

### 3.1.2 Email Packet (unencrypted)

Storage format for the `Incomplete Email`

| Field   | Data Type  | Description                                   |
|---------|------------|-----------------------------------------------|
| `TYPE`  | 1 byte     | Value = `'U'`                                 |
| `VER`   | 1 byte     | Protocol version                              |
| `MSID`  | 32 bytes   | Message ID in binary format                   |
| `DA`    | 32 bytes   | Delete authorization key, randomly generated  |
| `FRID`  | 2 bytes    | Fragment Index of this Packet (0..`NFR`-1)    |
| `NFR`   | 2 bytes    | Number of fragments in the email              |
| `MLEN`  | 2 bytes    | Length of the `CALG+MSG` fields               |
| `CALG`  | 1 byte     | Compression algorithm (see below)             |
| `MSG`   | byte[]     | email content (`MLEN` bytes)                  |

#### Compression algorithms

Supported algorithms may differ depending on the implementation. 

| Value | Description    |
|-------|----------------|
| `0`   | Uncompressed   |
| `1`   | LZMA           |
| `2`   | ZLIB `[VER 5]` |

### 3.1.3 Index Packet

Contains the DHT keys of one or more `Email Packets`, a Delete Verification Hash, and UNIX timestamp for each DHT key.

| Field   | Data Type  | Description                                                                  |
|---------|------------|------------------------------------------------------------------------------|
| `TYPE`  | 1 byte     | Value = `'I'`                                                                |
| `VER`   | 1 byte     | Protocol version                                                             |
| `DH`    | 32 bytes   | SHA-256 hash of the recipient's email destination                            |
| `NP`    | 4 bytes    | Number of entries in the Packet                                              |
| `KEY1`  | 32 bytes   | DHT key of the first Email Packet                                            |
| `DV1`   | 32 bytes   | Delete verification hash for `KEY1` (SHA-256 hash of DA in the email Packet) |
| `TIM1`  | 4 bytes    | The time the key `KEY1` was added                                            |
| `KEY2`  | 32 bytes   | DHT key of the second Email Packet                                           |
| `DV2`   | 32 bytes   | Delete verification hash for `KEY2` (SHA-256 hash of DA in the email Packet) |
| `TIM2`  | 4 bytes    | The time the key `KEY2` was added                                            |
| ...     | ...        | ...                                                                          |
| `KEYn`  | 32 bytes   | DHT key of the n-th Email Packet                                             |
| `DVn`   | 32 bytes   | Delete verification hash for `KEYn` (SHA-256 hash of DA in the email Packet) |
| `TIMn`  | 4 bytes    | The time the key `KEYn` was added                                            |

### 3.1.4 Deletion Info Packet

Contains information about deleted  DHT items, which can be `Email Packets` or `Index Packet` entries.

| Field   | Data Type  | Description                       |
|---------|------------|-----------------------------------|
| `TYPE`  | 1 byte     | Value = `'T'`                     |
| `VER`   | 1 byte     | Protocol version                  |
| `NP`    | 4 bytes    | Number of entries in the Packet   |
| `KEY1`  | 32 bytes   | First DHT key                     |
| `DA1`   | 32 bytes   | Delete Authorization for `KEY1`   |
| `TIM1`  | 4 bytes    | The time the key `KEY1` was added |
| `KEY2`  | 32 bytes   | Second DHT key                    |
| `DA2`   | 32 bytes   | Delete Authorization for `KEY2`   |
| `TIM2`  | 4 bytes    | The time the key `KEY2` was added |
| ...     | ...        | ...                               |
| `KEYn`  | 32 bytes   | The n-th DHT key                  |
| `DAn`   | 32 bytes   | Delete Authorization for `KEYn`   |
| `TIMn`  | 4 bytes    | The time the key `KEYn` was added |

### 3.1.5 Peer List

Response to a `Find Close Peers Request` or a `Peer List Request`.

| Field   | Data Type  | Description                                   |
|---------|------------|-----------------------------------------------|
| `TYPE`  | 1 byte     | Value = `'L'`                                 |
| `VER`   | 1 byte     | Protocol version                              |
| `NUMP`  | 2 bytes    | Number of peers in the list                   |
| `IDN1`  | 384 bytes  | Standart I2P identity `[VER 5]`               |
| `TYPE1` | 1 byte     | Certificate type `[VER 5]`                    |
| `LEN1`  | 2 byte     | Length of extra bytes `DATA1` field `[VER 5]` |
| `DATA1` | byte[]     | Extra bytes `[VER 5]`                         |
| `IDN2`  | 384 bytes  | Standart I2P identity `[VER 5]`               |
| `TYPE2` | 1 byte     | Certificate type `[VER 5]`                    |
| `LEN2`  | 2 byte     | Length of extra bytes `DATA2` field `[VER 5]` |
| `DATA2` | byte[]     | Extra bytes `[VER 5]`                         |
| ...     | ...        | ...                                           |
| `IDNn`  | 384 bytes  | Standart I2P identity `[VER 5]`               |
| `TYPEn` | 1 byte     | Certificate type `[VER 5]`                    |
| `LENn`  | 2 byte     | Length of extra bytes `DATAn` field `[VER 5]` |
| `DATAn` | byte[]     | Extra bytes `[VER 5]`                         |

### 3.1.6 Directory Entry (Contact)

A name/Email Destination mapping.

| Field   | Data Type  | Description                                                   |
|---------|------------|---------------------------------------------------------------|
| `TYPE`  | 1 byte     | Value = `'C'`                                                 |
| `VER`   | 1 byte     | Protocol version                                              |
| `KEY`   | 32 bytes   | DHT key (SHA-256 hash of the UTF8-encoded name in lower case) |
| `DLEN`  | 2 bytes    | Length of the `DEST` field                                    |
| `DEST`  | byte[]     | Email destination, 8 bit encoded (not base64)                 |
| `SALT`  | 4 bytes    | A value such that scrypt(KEY|DEST|SALT) ends with a zero byte |
| `PLEN`  | 2 bytes    | Length of `PIC`, max 8192 `[VER 5]`                           |
| `PIC`   | byte[]     | User picture in a browser-compatible format (JPG, PNG, GIF)   |
| `COMP`  | 1 byte     | Compression type for `TEXT`                                   |
| `TLEN`  | 2 bytes    | Length of the `TEXT` field, max 2048 `[VER 5]`                |
| `TEXT`  | byte[]     | User defined text in UTF8                                     |

## 3.2. Communication Packets

Communication packets are used for sending data between I2P-Bote nodes.   
They can contain a `Data Packet`, see 3.1.   
All Communication Packets start with a four-byte `Prefix`, followed by one byte for the `Packet type`, one byte for the `Packet version`, and a 32-byte `Correlation ID`.   

### 3.2.1 Relay Request

A Packet that tells the receiver to communicate with a peer, or peers, on behalf of the sender.    
It contains an I2P destination, a delay time, and a `Communication Packet`.    


| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'R'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |
| `HLEN`  | 2 bytes    | HashCash length                              |
| `HK`    | byte[]     | HashCash token (`HLEN` bytes)                |
| `DLY`   | 4 bytes    | Delay in seconds before sending              |
| `NEXT`  | 384 bytes  | Destination to forward the Packet to         |
| `RET`   | Ret. Chain | An empty or non-empty Return Chain           |
| `DLEN`  | 2 bytes    | Length of the `DATA` field in bytes          |
| `DATA`  | byte[] `E` | Encrypted with the recipient's public key. Can contain another Relay Request, a Store Request, or a Deletion Query. |
| `PAD`   | byte[]     | Optional padding                             |

### 3.2.2 Relay Return Request

Contains a `Return Chain` and a payload. The final destination is unknown to the sender or any of the hops.   
Used for responding to a `Relayed Data Packet`.

| Field   | Data Type  | Description                                            |
|---------|------------|--------------------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9`           |
| `TYPE`  | 1 byte     | Value = `'K'`                                          |
| `VER`   | 1 byte     | Protocol version                                       |
| `CID`   | 32 bytes   | Correlation ID, used for confirmation between two relay peers. A new correlation ID is set at every hop. |
| `RET`   | Ret. Chain | A non-empty Return Chain                               |
| `DLEN`  | 2 bytes    | Length of the `DATA` field in bytes                    |
| `DATA`  | byte[] `E` | The payload. AES256-encrypted at every hop in the return chain. |

### 3.2.3 Return Chain

Not a Packet itself, but part of a `Relay (Return) Request`.

| Field   | Data Type      | Description                                                          |
|---------|----------------|----------------------------------------------------------------------|
| `RL1`   | 2 bytes        | Length of `RET1`. Zero means an empty return chain, nothing follows. |
| `RET1`  | `RL1` bytes    | Hop 1 in the return chain. Contains a zero byte, followed by the hop's I2P destination and an AES key to encrypt the payload with. |
| `RL2`   | 2 bytes `E`    | Length of `RET2`                                                     |
| `RET2`  | `RL2` bytes `E`| Hop 2 in the return chain. Contains a zero byte, followed by the hop's I2P destination and an AES key to encrypt the payload with. This field is encrypted with the PK of hop 2. |
| `RL3`   | 2 bytes   `E`  | Length of `RET3`                                                     |
| `RET3`  | `RL3` bytes `E`| Hop 3 in the return chain. Contains a zero byte, followed by the hop's I2P destination and an AES key to encrypt the payload with. This field is encrypted with the PKs of hops 3 and 2. |
| ...   | ...              | ...                                                                  |
| `RLn`   | 2 bytes   `E`  | Length of `RETn`                                                     |
| `RETn`  | `RLn` bytes `E`| Last hop in the return chain. Contains a zero byte, followed by the hop's I2P destination and an AES key to encrypt the payload with. This field is encrypted with the PKs of hops n...2. |
| `RLn+1` | 2 bytes   `E`  | Length of `RETn+1`                                                   |
| `RETn+1`| `RLn+1` byt `E`| Number of hops (>0) followed by all AES256 keys (one per hop)        |
| `RLn+2` | 2 bytes        | Value=0 to indicate this is the end of the return chain              |

### 3.2.4 Fetch Request

Request to a chain endpoint to retrieve an `Index Packet` or `Email Packet`.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                            |
|---------|------------|--------------------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9`           |
| `TYPE`  | 1 byte     | Value = `'G'` `[VER 5]`                                |
| `VER`   | 1 byte     | Protocol version                                       |
| `CID`   | 32 bytes   | Correlation ID, used for responses                     |
| `DTYP`  | 1 byte     | Type of data to retrieve                               |
| `KEY`   | 32 bytes   | DHT key to look up                                     |
| `KPR`   | 384 bytes  | Email keypair of recipient                             |
| `RLEN`  | 2 bytes    | Length of the `RET` field                              |
| `RET`   | byte[]     | Relay Packet, contains the return chain (`RLEN` bytes) |

### 3.2.5 Response Packet

Response to a `Retrieve Request`, `Fetch Request`, `Find Close Peers Request`, or `Peer List Request`.

| Field   | Data Type  | Description                                        |
|---------|------------|----------------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9`       |
| `TYPE`  | 1 byte     | Value = `'N'`                                      |
| `VER`   | 1 byte     | Protocol version                                   |
| `CID`   | 32 bytes   | Correlation ID of the request Packet               |
| `STA`   | 1 byte     | Status code (see below)                            |
| `DLEN`  | 2 bytes    | Length of the `DATA` field; can be 0 if no payload |
| `DATA`  | byte[]     | A Data Packet                                      |

#### Status codes

| Value   | Description                  |
|---------|------------------------------|
| `0`     | OK                           |
| `1`     | General error                |
| `2`     | No data found                |
| `3`     | Invalid Packet               |
| `4`     | Invalid HashCash             |
| `5`     | Not enough HashCash provided |
| `6`     | No disk space left           |
| `7`     | Duplicated data `[VER 5]`    |

### 3.2.6 Peer List Request

A request for a list of high-reachability relay peers.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'A'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |

## 3.3 DHT Communication Packets

### 3.3.1 Retrieve Request

A request to a peer to return a data item for a given DHT key and data type.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'Q'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |
| `DTYP`  | 1 byte     | Type of data to retrieve (see below)         |
| `KEY`   | 32 bytes   | DHT key to look up                           |

#### Type of data to retrieve

| Value | Type of data    |
|-------|-----------------|
| `'I'` | Index Packet    |
| `'E'` | Email Packet    |
| `'C'` | Directory Entry |

### 3.3.2 Deletion Query

A request to a peer to return a `Deletion Info Packet` for a given `Email Packet` DHT key.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'Y'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |
| `KEY`   | 32 bytes   | DHT key to look up                           |

### 3.3.3 Store Request

DHT Store Request.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'S'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |
| `HLEN`  | 2 bytes    | HashCash length                              |
| `HK`    | byte[]     | HashCash token (`HLEN` bytes)                |
| `DLEN`  | 2 bytes    | Length of the `DATA` field                   |
| `DATA`  | byte[]     | `Data Packet` to store (`DLEN` bytes). Can be an `Index Packet`, `Email Packet`, or `Email Destination`. |

### 3.3.4 Email Packet Delete Request

Request to delete an Email Packet by DHT key.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                                      |
|---------|------------|------------------------------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9`                     |
| `TYPE`  | 1 byte     | Value = `'D'`                                                    |
| `VER`   | 1 byte     | Protocol version                                                 |
| `CID`   | 32 bytes   | Correlation ID, used for responses                               |
| `KEY`   | 32 bytes   | DHT key of the Email Packet to delete                            |
| `DA`    | 32 bytes   | Delete Authorization (SHA-256 must equal DV in the email packet) |

### 3.3.5 Index Packet Delete Request

Request to remove one or more entries (Email Packet keys) from an `Index Packet`.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                                                           |
|---------|------------|---------------------------------------------------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9`                                          |
| `TYPE`  | 1 byte     | Value = `'X'`                                                                         |
| `VER`   | 1 byte     | Protocol version                                                                      |
| `CID`   | 32 bytes   | Correlation ID, used for responses                                                    |
| `DH`    | 32 bytes   | The Email Destination hash of the Index Packet                                        |
| `N`     | 1 byte     | Number of entries in the Packet                                                       |
| `DHT1`  | 32 bytes   | First DHT key to remove                                                               |
| `DA1`   | 32 bytes   | Delete Authorization (SHA-256 must equal DV in the email Packet referenced by `DHT1`) |
| `DHT2`  | 32 bytes   | Second DHT key to remove                                                              |
| `DA2`   | 32 bytes   | Delete Authorization (SHA-256 must equal DV in the email Packet referenced by `DHT2`) |
| ...     | ...        | ...                                                                                   |
| `DHTn`  | 32 bytes   | n-th DHT key to remove                                                                |
| `DAn`   | 32 bytes   | Delete Authorization (SHA-256 must equal DV in the email Packet referenced by `DHTn`) |

### 3.3.6 Find Close Peers

Request for k peers close to a key.    
`Response Packet` with correct status is expected back `[VER 5]`.

| Field   | Data Type  | Description                                  |
|---------|------------|----------------------------------------------|
| `PFX`   | 4 bytes    | Packet prefix, must be `0x6D 0x30 0x52 0xE9` |
| `TYPE`  | 1 byte     | Value = `'F'`                                |
| `VER`   | 1 byte     | Protocol version                             |
| `CID`   | 32 bytes   | Correlation ID, used for responses           |
| `KEY`   | 32 bytes   | DHT key                                      |
