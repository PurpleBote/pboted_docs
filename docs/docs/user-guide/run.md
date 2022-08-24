# Running

## pboted needs I2P router

- Install I2P router
- Enable SAMv3 API
- Restart I2P router   
- Local TCP port 7656 and UDP port 7655 should be available

## Recommended way to run (built from source)

- Create `/etc/pboted` directory and copy example config:

```
sudo mkdir /etc/pboted
sudo cp contrib/pboted.conf /etc/pboted/pboted.conf
```

- Edit the config to suit your needs. The file is well documented, comments will help you.
- Create user, data and logs directories:

```
sudo useradd pboted -r -s /usr/sbin/nologin
sudo mkdir /var/lib/pboted
sudo chown -R pboted: /var/lib/pboted
sudo mkdir /var/log/pboted
sudo chown -R pboted: /var/log/pboted
```

- Copy `logrotate` configuration file for logs rotation

```
sudo cp contrib/pboted.logrotate /etc/logrotate.d/pboted
```

- Copy example `systemd` service file, reload daemons configuration, and start unit:

```
sudo cp contrib/pboted.service /lib/systemd/system/pboted.service
sudo systemctl daemon-reload
sudo systemctl start pboted.service
```

- Now you can see in log files that all works. Also, you can see the status of the SAM session in the I2P Router console.

## Running in userspace

- Create `~/.pboted` directory and сopy example config from `contrib/pboted.conf` to `~/.pboted/pboted.conf`:

```
mkdir ~/.pboted
cp contrib/pboted.conf ~/.pboted/pboted.conf
```

- Edit the config to suit your needs. The file is well documented, comments will help you.
- Now you can run application:

```
./pboted
```
