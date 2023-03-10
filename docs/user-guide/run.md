# Running

## pboted needs I2P router

- Install I2P router
- Enable SAMv3 API
- Restart I2P router   
- Local TCP port 7656 and UDP port 7655 should be available

## Running as system service (recommended)

- Create `/etc/pboted` directory and copy example config:

```bash
sudo mkdir /etc/pboted
sudo cp contrib/pboted.conf /etc/pboted/pboted.conf
```

- Edit the config to suit your needs. The file is well documented, comments will help you.
- Create user, data and logs directories:

```bash
sudo useradd pboted -r -s /usr/sbin/nologin
sudo mkdir /var/lib/pboted
sudo chown -R pboted: /var/lib/pboted
sudo mkdir /var/log/pboted
sudo chown -R pboted: /var/log/pboted
```

- Copy `logrotate` configuration file for logs rotation

```bash
sudo cp contrib/pboted.logrotate /etc/logrotate.d/pboted
```

- Copy example `systemd` unit file, reload daemons configuration, and start unit:

```bash
sudo cp contrib/pboted.service /lib/systemd/system/pboted.service
sudo systemctl daemon-reload
sudo systemctl start pboted.service
```

- Now you can see in log files that all works. Also, you can see the status of the SAM session in the I2P Router console.

## Running in userspace

- Create `~/.pboted` directory and сopy example config from `contrib/pboted.conf` to `~/.pboted/pboted.conf`:

```bash
mkdir ~/.pboted
cp contrib/pboted.conf ~/.pboted/pboted.conf
```

- Edit the config to suit your needs. The file is well documented, comments will help you.
- Copy example `systemd` unit file and edit paths to pboted binary, home dir, etc.):

```bash
mkdir -p ~/.config/systemd/user
cp contrib/pboted.service ~/.config/systemd/user/pboted.service
```

- Unit file example for user `user`:

```ini
[Unit]
Description=I2P/Bote service written in C++
Documentation=man:pboted(1) https://pboted.readthedocs.io/en/latest/
After=network.target

[Service]
RuntimeDirectoryMode=0700
LogsDirectoryMode=0700
Type=forking

ExecStart=/home/user/.local/bin/pboted --log=file --daemon
ExecReload=/bin/sh -c "kill -HUP $MAINPID"

PIDFile=/home/user/.pboted/pboted.pid

# Use SIGTERM to stop pboted immediately.
KillSignal=SIGTERM
TimeoutStopSec=3m
SendSIGKILL=yes

# To enable write of coredump uncomment this
#LimitCORE=infinity

[Install]
WantedBy=multi-user.target
```

- Reload services configuration:

```bash
systemctl --user daemon-reload
```

- Now you able to start service and set it to autostart:

```bash
systemctl --user start pboted.service
systemctl --user enable pboted.service
```

## Simple way

- Create `~/.pboted` directory and сopy example config from `contrib/pboted.conf` to `~/.pboted/pboted.conf`:

```bash
mkdir ~/.pboted
cp contrib/pboted.conf ~/.pboted/pboted.conf
```

- Edit the config to suit your needs. The file is well documented, comments will help you.
- Now you able to run application:

```bash
./pboted
```
