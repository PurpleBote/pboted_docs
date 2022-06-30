# Introduction (DRAFT)

I2P-Bote is a serverless pseudonymous email exchange service for the I2P network.  
Emails are stored encrypted in a Kademlia DHT formed by all I2P-bote nodes. There is a SMTP/POP3 interface that lets the user send/read email.

Email can be sent through a number of other nodes (relays) for increased anonymity, or directly to a set of storage nodes for faster delivery. Receiving email through relays is not yet implemented.

All nodes are created equal.
There are no "supernodes" or designated relay/storage nodes.
Everybody acts as a potential relay and storage node.
The maximum amount of disk space used for relayed/stored email packets is user-configurable.
Before an email is sent, it is broken up into packets 30 kb or smaller and encrypted with the recipient's public key.
The packets are then stored in the DHT.

Email packets are stored redundantly in a Kademlia DHT (distributed hash table).   
Stored email packets and relay packets are kept for at least 100 days, during which the recipient can download them. If a node runs out of email storage space, and there are no old packets that can be deleted, the node refuses storage requests.

Below is a diagram of how an email Packet is routed from a sender to a recipient:

```
                    .-------.        .-------.        .--------.
                    | Relay |  --->  | Relay |  --->  | Storer |  --------------------.
                _   `-------'        `-------'        `--------'                       `\
                /`                                                                       |
               /                                                                         |
 .--------.   /     .-------.        .-------.         .--------.                        |
 | Sender |  ---->  | Relay |  --->  | Relay |  --->  | Storer |  -------------.         |
 `--------'   \     `-------'        `-------'         `--------'               `\       |
               \                                                                  |      |
               _\/                                                                |      |
                    .-------.        .-------.        .--------.                  |      |
                    | Relay |  --->  | Relay |  --->  | Storer |  ------.         |      |
                    `-------'        `-------'        `--------'         `\       |      |
                                                                           |      |      |
                                                                           V      V      V

                                        .--------------- Kademlia DHT -------------------------.
                                        |                                .---------.           |
                                        |   .---------.                  | Storage |           |
                                        |   | Storage |     .---------.  |  Node   |           |
                                        |   |  Node   |     | Storage |  `---------'           |
                                        |   `---------'     |  Node   |                        |
                                        |                   `---------'            .---------. |
                                        |  .---------.   .---------.               | Storage | |
                                        |  | Storage |   | Storage |  .---------.  |  Node   | |
                                        |  |  Node   |   |  Node   |  | Storage |  `---------' |
                                        |  `---------'   `---------'  |  Node   |              |
                                        |                             `---------'              |
                                        `------------------------------------------------------'

                                                                          |      |      |
                     .-------.        .-------.        .---------.       /       |      |
                     | Relay |  <---  | Relay |  <---  | Fetcher |  <---'        |      |
                  /  `-------'        `-------'        `---------'               |      |
                 /                                                               |      |
               \/_                                                               |      |
 .-----------.       .-------.        .-------.        .---------.              /       |
 | Recipient |  <--  | Relay |  <---  | Relay |  <---  | Fetcher |  <----------'        |
 `-----------'  _    `-------'        `-------'        `---------'                      |
               |\                                                                       |
                 \                                                                      |
                  \  .-------.        .-------.        .---------.                     /
                     | Relay |  <---  | Relay |  <---  | Fetcher |  <-----------------'
                     `-------'        `-------'        `---------'
```

The number of relays for sending or retrieving an email is user-configurable.

For higher performance (but reduced anonymity), it is possible to use no relays and no storers/fetchers, i.e. sender and recipient talk to the storage nodes directly.   
If both sender and recipient chose not to use relays, the diagram looks like this:

```
 .--------.
 | Sender |  ----------------------------.
 `--------'                               `\
                                            |
                                            V

               .--------------- Kademlia DHT -------------------------.
               |                                .---------.           |
               |   .---------.                  | Storage |           |
               |   | Storage |     .---------.  |  Node   |           |
               |   |  Node   |     | Storage |  `---------'           |
               |   `---------'     |  Node   |                        |
               |                   `---------'            .---------. |
               |  .---------.   .---------.               | Storage | |
               |  | Storage |   | Storage |  .---------.  |  Node   | |
               |  |  Node   |   |  Node   |  | Storage |  `---------' |
               |  `---------'   `---------'  |  Node   |              |
               |                             `---------'              |
               `------------------------------------------------------'

                                            |
                                            |
 .-----------.                             /
 | Recipient |  <-------------------------'
 `-----------'
```

I2P-Bote uses base64 strings for addresses. They are called [Email Destinations](terms.md#email-destination) and can be between 86 and 512 characters long, depending on the type of encryption the user chooses.  
For more details see [Cryptography](cryptography.md).
