# Файлы

## Файл идентичностей

В этом файле хранятся все `Почтовые идентичности`, используемые во время работы.  
Используется формат `Java Properties`.

В настоящее время сохраняются и распознаются следующие параметры (N — целочисленный индекс):

| Field                | Description                                                                      |
|----------------------|----------------------------------------------------------------------------------|
|identityN.publicName  | Публичное имя, добавляемое в письма                                              |
|identityN.key         | `Почтовая идентичность`                                                          |
|identityN.salt        | Соль, используемая для создания отпечатков                                       |
|identityN.description | Описание `Почтовой идентичности`, используется только локально                   |
|identityN.picture     | Аватар в совместимом с браузером формате (JPG, PNG, GIF), закодированый в Base64 |
|identityN.text        | Текст, связанный с `Почтовой идентичностью`                                      |
|identityN.published   | Будет ли `Почтовая идентичность` публиковаться?                                  |

Подробнее о форматах `Почтовой идентичности` Вы можете узнать в разделе [Криптография](https://bote.readthedocs.io/en/latest/v5/cryptography#2) протокола I2P Bote.

Файл может содержать ключ `default`, со значением `Почтового назначения`.

## Адресная книга

See [Адресная книга](addressbook.ru.md).

## Шифрование паролем

The identities file, the address book, and all email files are encrypted with AES-256.   
To generate the AES key from the password, scrypt (http://www.tarsnap.com/scrypt.html) is used.   
The file format is:

| Field | #bytes | Description                                   |
|-------|--------|-----------------------------------------------|
| SOF   | 4      | Start of file, contains the characters "IBef" |
| VER   | 1      | Format version, must be 1                     |
| N     | 4      | scrypt CPU cost parameter                     |
| r     | 4      | scrypt memory cost parameter                  |
| p     | 4      | scrypt parallelization parameter              |
| SALT  | 32     | Salt for scrypt                               |
| IV    | 32     | IV for AES                                    |
| DATA  |        | The encrypted data                            |

Parameters N through SALT are cached in a file named derivparams so all encrypted files can use the same key derivation parameters.   
This makes decryption much faster because the key only needs to be derived once per session.
