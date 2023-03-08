# Запуск

## pboted требуется I2P маршрутизатор

- Установите I2P маршрутизатор
- Включите SAMv3 API
- Перезапустите I2P маршрутизатор
- TCP порт 7656 и UDP порт 7655 должны стать доступны

## Запуск в роли системного сервиса (рекомендуется)

- Создайте директорию `/etc/pboted` и скопируйте образец конфигурационного файла:

```bash
sudo mkdir /etc/pboted
sudo cp contrib/pboted.conf /etc/pboted/pboted.conf
```

- Отредайтируйте конфигурационный файл под Ваши нужды. Образец файла хорошо документирован, комментарии помогут Вам в понимании параметров.
- Создайте пользователя, директории для данных и журналов:

```bash
sudo useradd pboted -r -s /usr/sbin/nologin
sudo mkdir /var/lib/pboted
sudo chown -R pboted: /var/lib/pboted
sudo mkdir /var/log/pboted
sudo chown -R pboted: /var/log/pboted
```

- Скопируйте образец `logrotate` конфигурации для ротации логов:

```bash
sudo cp contrib/pboted.logrotate /etc/logrotate.d/pboted
```

- Скопируйте образец `systemd` Unit-файла, перезагрузите конфигурацию `systemd` сервисов и запустите приложение:

```bash
sudo cp contrib/pboted.service /lib/systemd/system/pboted.service
sudo systemctl daemon-reload
sudo systemctl start pboted.service
```

- Теперь Вы сможете наблюдать работу приложения по записям в журнале. SAM сессия будет отображатся в Web консоли I2P маршрутизатора.

## Запуск в пространстве пользователя

- Создайте директорию `~/.pboted` и скопируйте образец файла конфигурации из `contrib/pboted.conf` в `~/.pboted/pboted.conf`:

```bash
mkdir ~/.pboted
cp contrib/pboted.conf ~/.pboted/pboted.conf
```

- Отредайтируйте конфигурационный файл под Ваши нужды. Образец файла хорошо документирован, комментарии помогут Вам в понимании параметров.
- Скопируйте образец `systemd` Unit-файла и отредактируйте пути:

```bash
mkdir -p ~/.config/systemd/user
cp contrib/pboted.service ~/.config/systemd/user/pboted.service
```

- Пример Unit-файла для пользователя `user`:

```ini
[Unit]
Description=I2P-Bote service written in C++
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

- Перезагрузите конфигурацию `systemd` сервисов:

```bash
systemctl --user daemon-reload
```

- Теперь вы можете запустить приложение как сервис пользователя и также добавить его в автозапуск:

```bash
systemctl --user start pboted.service
systemctl --user enable pboted.service
```

## Простой запуск

- Создайте директорию `~/.pboted` и скопируйте образец файла конфигурации из `contrib/pboted.conf` в `~/.pboted/pboted.conf`:

```bash
mkdir ~/.pboted
cp contrib/pboted.conf ~/.pboted/pboted.conf
```

- Отредайтируйте конфигурационный файл под Ваши нужды. Образец файла хорошо документирован, комментарии помогут Вам в понимании параметров.
- Запустите приложение:

```bash
./pboted
```
