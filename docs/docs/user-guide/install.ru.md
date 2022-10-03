# Установка

## Debian/Ubuntu

Вы можете установить готовый пакет со [страницы последнего релиза](https://github.com/polistern/pboted/releases/latest).

## Сборка из исходного кода

### Требования

Для сборки **pboted** Вам необходимы:

* компилятор с поддержкой страндарта c++17 (например: gcc >= 5, clang >= 5)
* cmake >= 3.7
* mimetic >= 0.9.8
* boost >= 1.62 (filesystem и program_options)
* openssl >= 1.1.1
* zlib (openssl уже имеет её в зависимостях)

## Сборка на UNIX-подобных системах

**Поддерживаемые системы:**

* GNU/Linux
    - [Debian/Ubuntu](#debian-ubuntu) (с созданием пакета)
    - CentOS/RedHat

Убедитесь, что все требуемые зависимости установлены.  
Ознакомтесь с зависимостями в [этой](#Требования) секции.

- Клонируйте репозиторий:

```bash
git clone https://github.com/polistern/pboted.git
cd pboted
```

- Скомпилируйте:

```bash
cd build
cmake <cmake options> . # Смотри секцию "Опции CMake" ниже
make                    # Для отладки можно добавить VERBOSE=1
```

### Опции CMake

Доступные опции CMake (каждая опция имеет вид `-D<key>=<value>`, больше информации в `man 1 cmake`):

- `CMAKE_BUILD_TYPE` тип сборки (Debug/Release, по умолчанию: без оптимизаций и отладки)
- `WITH_STATIC`      статичная сборка pboted (по умолчанию: OFF)

Для CMake доступен флаг `-L` для просмотра текущих опций сборки:

```bash
cmake -L
```

#### Принудительно использовать GCC

```bash
CC=gcc CXX=g++ cmake .
```

#### Принудительно использовать Clang

```bash
CC=clang CXX=clang++ cmake .
```

### Debian/Ubuntu:

!!! note "Примечание"

    Проверено на Debian 10 и Ubuntu 20.04

Вам необходимы компилятор и другие утилиты, которые могут быть установлены с пакетами `build-essential` и `debhelper`:

```bash
sudo apt install git cmake build-essential debhelper
```

Также вам понадобятся библиотеки разработки:

```bash
sudo apt install \
  libboost-filesystem-dev \
  libboost-program-options-dev \
  libmimetic-dev \
  libssl-dev \
  pkg-config \
  zlib1g-dev
```

Доступна возможность собрать DEB пакет самостоятельно:

```bash
sudo apt install fakeroot devscripts dh-apparmor
cd pboted
debuild --no-tgz-check -us -uc -b
```
