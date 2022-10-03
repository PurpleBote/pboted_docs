# Installing

## Debian/Ubuntu

You can install binary packages from the [latest release page](https://github.com/polistern/pboted/releases/latest).

## Building from source

### Requirements

For building **pboted** you need several things:

* compiler with c++17 support (for example: gcc >= 5, clang >= 5)
* cmake >= 3.7
* mimetic >= 0.9.8
* boost >= 1.62 (filesystem and program_options)
* openssl >= 1.1.1
* zlib (openssl already depends on it)

## Building on UNIX-like systems

**Supported systems:**

* GNU/Linux
    - [Debian/Ubuntu](#debian-ubuntu) (with packaging)
    - CentOS/RedHat

Make sure you have all required dependencies successfully installed.  
See for common requirements in [this](#requirements) section.

- Clone repository:

```bash
git clone https://github.com/polistern/pboted.git
cd pboted
```

- Build:

```bash
cd build
cmake <cmake options> . # See "CMake Options" section below
make                    # You may add VERBOSE=1 to cmdline for debugging
```

### CMake Options

Available CMake options(each option has a form of `-D<key>=<value>`, for more information see `man 1 cmake`):

* `CMAKE_BUILD_TYPE` build profile (Debug/Release, default: no optimization or debug symbols)
* `WITH_STATIC`      build static pboted binary (default: OFF)

Also there is `-L` flag for CMake that could be used to list current cached options:

```bash
cmake -L
```

#### Force GCC

```bash
CC=gcc CXX=g++ cmake .
```

#### Force Clang

```bash
CC=clang CXX=clang++ cmake .
```

### Debian/Ubuntu:

!!! note "Note"

    Tested with Debian 10 and Ubuntu 20.04

You will need a compiler and other tools that could be installed with `build-essential` and `debhelper` packages:

```bash
sudo apt install git cmake build-essential debhelper
```

Also you will need a bunch of development libraries:

```bash
sudo apt install \
  libboost-filesystem-dev \
  libboost-program-options-dev \
  libmimetic-dev \
  libssl-dev \
  pkg-config \
  zlib1g-dev
```

You may also build DEB-package with the following:

```bash
sudo apt install fakeroot devscripts dh-apparmor
cd pboted
debuild --no-tgz-check -us -uc -b
```
