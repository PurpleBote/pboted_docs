# pboted

pboted (Plus Bote Daemon) - is a standalone C++ implementation of I2P-Bote protocol.

I2P-Bote is a server-less encrypted KademliaDHT-based email protocol.   
You can find more details it [Bote](bote/v5/version5.md) section.

Interaction with the I2P network occurs through the [SAMv3](https://geti2p.net/ru/docs/api/samv3) interface.
Tested with [i2pd](https://github.com/PurpleI2P/i2pd) and [Java I2P](https://github.com/i2p/i2p.i2p).

## Alpha

Please note that **pboted** version **0.7.X** is still in **alpha**.
During this period, there may be significant changes in the application.

Transition to **beta** planned in version **0.9.X**

## Features

- Sending and receiving emails
- Support for short recipient names (alias)
- End-to-End encryption ([details](bote/v5/cryptography/))
- Runnable as daemon
- [CLI utility](https://github.com/polistern/pbotectl) (work in progress)
- SMTP / POP3 support (tested with [Mozilla Thunderbird](https://www.thunderbird.net/en-US/))

## Planned Features

- Custom per identity/user email folders
- Sending email anonymously
- Delivery confirmation
- Sending and receiving via relays, similar to Mixmaster
- Interfaces for interaction with third-party applications (IMAP, etc.)

## Resources

- [Documentation](https://pboted.readthedocs.io/en/latest/)
- [Tickets/Issues](https://github.com/polistern/pboted/issues)
