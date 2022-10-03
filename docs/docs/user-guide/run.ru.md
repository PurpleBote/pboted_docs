# Запуск

## pboted требуется I2P маршрутизатор

- Установите I2P маршрутизатор
- Включите SAMv3 API
- Перезапустите I2P маршрутизатор
- TCP порт 7656 и UDP порт 7655 должны стать доступны

## Рекомендуемый способ запуска (собранного из исходников)

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
- Запустите приложение:

```bash
./pboted
```
