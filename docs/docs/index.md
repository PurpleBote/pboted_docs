# pboted

* [clearnet](https://pboted.readthedocs.io/en/latest/)
* [I2P](http://polistern.i2p/pbote/)

**pboted** (_Plus Bote Daemon_) - is a standalone C++ implementation of `I2P-Bote` protocol.

I2P-Bote is a server-less encrypted [DHT Kademlia](https://en.wikipedia.org/wiki/Distributed_hash_table)-based email protocol.   
You can find more details it [Bote](bote/v5/version5.md) section.

Interaction with the I2P network occurs through the I2P router [SAMv3](https://geti2p.net/en/docs/api/samv3) interface.
Tested with I2P routers [i2pd](https://github.com/PurpleI2P/i2pd) and [Java I2P](https://github.com/i2p/i2p.i2p).

## Alpha

Please note that **pboted** version **0.7.X** is still in **alpha**.
During this period, there may be significant changes in the application.

Transition to **beta** planned in version **0.9.X**

## Features

- Sending and receiving emails
- Support for short recipient names (alias)
- [End-to-End encryption](bote/v5/cryptography/)
- Runnable as UNIX daemon and Windows Service
- Built-in SMTP and POP3 (tested with [Mozilla Thunderbird](https://www.thunderbird.net/en-US/))
- Delivery confirmation
- [CLI utility](https://github.com/polistern/pbotectl) (work in progress)

### Planned Features

- Custom per identity/user email folders (WIP)
- Sending and receiving via relays (similar to Mixmaster)
- Sending email anonymously

## Resources

- [Documentation](https://pboted.readthedocs.io/en/latest/)
- [Tickets/Issues](https://github.com/polistern/pboted/issues)
