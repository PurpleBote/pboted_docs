# pboted

* [clearnet](https://pboted.readthedocs.io/en/latest/)
* [I2P](http://polistern.i2p/pbote/)

**pboted** (_Plus Bote Daemon_) - is a standalone C++ implementation of `I2P-Bote` protocol.

I2P-Bote is a server-less encrypted [DHT Kademlia](https://en.wikipedia.org/wiki/Distributed_hash_table)-based email protocol.   
You can find more details about Bote protocol [here](https://bote.readthedocs.io/en/latest/).

Interaction with the I2P network occurs through the I2P router [SAMv3](https://geti2p.net/en/docs/api/samv3) interface.
Tested with I2P routers [i2pd](https://github.com/PurpleI2P/i2pd) and [Java I2P](https://github.com/i2p/i2p.i2p).

## Alpha

Please note that **pboted** version **0.7.X** is still in **alpha**.
During this period, there may be significant changes in the application.

Transition to **beta** planned in version **0.9.X**

## Features

- Sending and receiving emails
- Support for short recipient names (alias)
- [End-to-End encryption](https://bote.readthedocs.io/en/latest/v5/cryptography/)
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

## Donations

The project is not intended to generate commercial benefits.

- **XMR**: `85P3aEXrYMn1YxnQaZSBWy6Ur6j9PVRxmCd3Ey1UanKAdKnhd2iYNdrEhNJ2JeUdcC8otSHogRTnydn4aMh8DwbSMs4N13Z`

## License

This project is licensed under the BSD 3-clause license, which can be found in the file `LICENSE` in the root of the project source code.

## Contributing

See file `CONTRIBUTING.md` in the root of the project source code.

## Special thanks

* [orignal](https://github.com/orignal) - for mentoring
* [R4SAS](https://github.com/r4sas) - for the help at the start
