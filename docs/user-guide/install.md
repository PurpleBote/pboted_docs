# Installing

## Debian/Ubuntu

You can install binary packages from the [latest release page](https://github.com/polistern/pboted/releases/latest).

## Building from source

### Requirements

For building **pboted** you need several things:

* compiler with c++17 support (for example: gcc >= 5, clang >= 5)
* cmake >= 3.7
* mimetic >= 0.9.8
* boost >= 1.62 (program_options)
* openssl >= 1.1.1
* zlib (openssl already depends on it)

## Building on UNIX-like systems

**Supported systems:**

* GNU/Linux
    - [Debian/Ubuntu](#debian-ubuntu) (with packaging)
    - [CentOS/Fedora] (#centos-fedora) (Fedora 36)

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

| Option             | Description                       | Default                              |
|--------------------|-----------------------------------|--------------------------------------|
| `CMAKE_BUILD_TYPE` | Build profile (Debug/Release)     | No optimization or debug symbols     |
| `CONTROL_SOCKET`   | Use UNIX socket in control module | `ON`                                 |
| `WITH_STATIC`      | Build static pboted binary        | `OFF`                                |
| `WITH_CXX11`       | Force C++11                       | `OFF` and higher available from both |
| `WITH_CXX17`       | Force C++17                       | `OFF` and higher available from both |

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

### Debian/Ubuntu

!!! note "Note"

    Tested with Debian 10 and Ubuntu 20.04

You will need a compiler and other tools that could be installed with `build-essential` and `debhelper` packages:

```bash
sudo apt install git cmake build-essential debhelper
```

Also you will need a bunch of development libraries:

```bash
sudo apt install \
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

### CentOS/Fedora

!!! note "Note"

    Tested with Fedora 36

Install required tools and libraries:

```bash
sudo dnf install cmake mimetic boost g++ boost-devel mimetic-devel
```


### Microsoft Windows

WIP
