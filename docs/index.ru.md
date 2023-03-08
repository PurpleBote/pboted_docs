# pboted

* [clearnet](https://pboted.readthedocs.io/en/latest/)
* [I2P](http://polistern.i2p/pbote/)

**pboted** (_Plus Bote Daemon_) - это реализация самостоятельного клиента для протокола `I2P-Bote` на языке C++.

I2P-Bote - это бессерверный, основанный на [распределённой хеш-таблице Kademlia](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D1%81%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D1%91%D0%BD%D0%BD%D0%B0%D1%8F_%D1%85%D0%B5%D1%88-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0), протокол обмена зашифрованными сообщениями электронной почты.   
Вы можете найти больше деталей о протоколе Bote [здесь](https://bote.readthedocs.io/en/latest/).

Взаимодействие с сетью осуществляется через API интерфейс I2P маршрутизатора [SAMv3](https://geti2p.net/ru/docs/api/samv3).
Совместимость проверена на I2P маршрутизаторах [i2pd](https://github.com/PurpleI2P/i2pd) и [Java I2P](https://github.com/i2p/i2p.i2p).

## Alpha

Обратите внимание, что **pboted** версии **0.7.X** всё ещё на стадии **alpha**.
В течении этого периода при релизах могут происходить значительные изменения со стороны приложения.

Переход на стадию **beta** планируется в версии **0.9.X**.

## Функционал

- Отправка и получение электронной почты
- Поддержка коротких имён получателя (псевдонимов)
- [Сквозное шифрование](https://bote.readthedocs.io/en/latest/v5/cryptography/)
- Запуск и работа в качестве UNIX-демона и Windows сервиса
- Встроенные SMTP и POP3 (проверено с [Mozilla Thunderbird](https://www.thunderbird.net/en-US/))
- Подтверждение доставки
- [CLI утилита](https://github.com/polistern/pbotectl) (в процессе разработки)

### Планируемый функционал

- Настраиваемые почтовые директории по идентичностям/пользователям (WIP)
- Отправка и получение писем через реле (подобно Mixmaster)
- Анонимная отправка писем

## Ресурсы

- [Документация](https://pboted.readthedocs.io/ru/latest/)
- [Заявки/Вопросы](https://github.com/polistern/pboted/issues)

## Пожертвования

Проект не имеет цели получения коммерческой выгоды.

- **XMR**: `85P3aEXrYMn1YxnQaZSBWy6Ur6j9PVRxmCd3Ey1UanKAdKnhd2iYNdrEhNJ2JeUdcC8otSHogRTnydn4aMh8DwbSMs4N13Z`

## Лицензия

Этот проект находится под лицензией BSD из 3-х пунктов, которую можно найти в файле `LICENSE` в корне исходного кода проекта.

## Содействие

Смотри файл `CONTRIBUTING.md` в корне исходного кода проекта.

## Особые благодарности

* [orignal](https://github.com/orignal) - за наставничество
* [R4SAS](https://github.com/r4sas) - за помощь на старте
