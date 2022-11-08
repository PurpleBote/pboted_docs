# Configuration

Run `pboted --help` to show builtin help message.

## General options

!!! note "Note"

    `service` option are only used as argument for pboted, these option have no effect when set in `pboted.conf`.

| Option        | Description                                                                                              | Default                                                |
|---------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| daemon        | pboted will go to background after start                                                                 | no                                                     |
| service       | pboted will use system folders like `/var/lib/pboted`                                                    | no                                                     |
| conf          | Config file. This parameter will be silently ignored if the specified config file does not exist.        | `~/.pboted/pboted.conf` or `/etc/pboted/pboted.conf`   |
| pidfile       | Where to write pidfile                                                                                   | `~/.pboted/pboted.pid` or `/var/lib/pboted/pboted.pid` |
| log           | Logs destination: `stdout`, `file`, `syslog` (if daemon, `stdout`/`unspecified` are replaced by `file` in some cases) | `stdout` if not set or invalid            |
| logfile       | Path to logfile                                                                                          | autodetect                                             |
| loglevel      | Log messages above this level (`debug`, `info`, `warn`, `error`, `none`)                                 | `info`                                                 |
| logclftime    | Write full CLF-formatted date and time to log (default: write only time)                                 | no                                                     |
| host          | External IP for incoming connections                                                                     | `0.0.0.0`                                              |
| port          | UDP port to listen for incoming connections                                                              | `5050`                                                 |
| storage       | Limit for local storage usage (`B`, `KiB`, `MiB`, `GiB`, `TiB`)                                          | `50 MiB`                                               |
| cleaninterval | Duration in days of node/peer unavailability after which it will be deleted                              | `7`                                                    |
| coresize      | Maximum size of corefile in KiB (0 - use system limit)                                                   | Use system limit                                       |
| openfiles     | Maximum number of open files (0 - use system limit)                                                      | Use system limit                                       |

## Control

| Option          | Description                                 | Default                                                |
|-----------------|---------------------------------------------|--------------------------------------------------------|
| control.enabled | Enable control                              | `true`                                                 |
| control.socket  | Path to control socket (caution in Windows) | `~/pboted/pboted.sock` or `/var/lib/pboted/pbote.sock` |
| control.address | Control listen address                      | `127.0.0.1`                                            |
| control.port    | Control listen TCP port                     | `5055`                                                 |

## SAM

| Option      | Description                              | Default                                                          |
|-------------|------------------------------------------|------------------------------------------------------------------|
| sam.name    | Nickname, showed in I2P router           | `pboted`                                                         |
| sam.address | I2P roter address with enabled SAM       | `127.0.0.1`                                                      |
| sam.tcp     | I2P SAM TCP port                         | `7656`                                                           |
| sam.udp     | I2P SAM UDP port                         | `7655`                                                           |
| sam.key     | Path to I2P destination private key file | `~/.pboted/destination.key` or `/var/lib/pboted/destination.key` |

## Bootstrap

!!! note "Note"

    The example configuration file has already added nodes for bootstrap

| Option            | Description                        | Default |
|-------------------|------------------------------------|---------|
| bootstrap.address | These are the nodes with high uptime and the most information about peers in the network. To get started, you need at least one node that supports protocol version 4 or higher. Each line should be a I2P destination key in Base64 format. | - |

You can specify this parameter multiple times with different addresses.  
For example: 

```ini
[bootstrap]
address = <first address>
address = <second address>
...
address = <N-th address>
```

## SMTP

| Option       | Description          | Default     |
|--------------|----------------------|-------------|
| smtp.enabled | Enable SMTP          | `true`      |
| smtp.address | SMTP listen address  | `127.0.0.1` |
| smtp.port    | SMTP listen TCP port | `9025`      |

## POP3

| Option       | Description          | Default     |
|--------------|----------------------|-------------|
| pop3.enabled | Enable POP3          | `true`      |
| pop3.address | POP3 listen address  | `127.0.0.1` |
| pop3.port    | POP3 listen TCP port | `9110`      |
