Building SSF for Unix
=====================

Get build dependencies
----------------------

SSF depends on Boost and OpenSSL libraries, as well as Kerberos headers.
Install development packages for these using your distro's package manager.

**NOTE**: Boost ASIO appears to be incompatible with OpenSSL 1.1, be sure
to install OpenSSL 1.0.2.

For example, on Debian/Ubuntu:

```
# apt-get install libssl1.0-dev libboost1.62 libboost-dev libkrb5-dev
```

As an alternative, you can also build OpenSSL and/or Boost yourself from
source. The `build_openssl.sh` and `build_boost.sh` scripts in the
`builddeps` folder can be used for that.

Building SSF
------------

Building SSF requires CMake and a C++ compiler.

On Debian/Ubuntu, these can be obtained using apt:

```
# apt-get install cmake g++
```

If you obtained the source for the git repository, make sure the submodules
are checked out:

```
$ git clone https://github.com/securesocketfunnelling/ssf.git
$ git submodule update --init
```

Create a build directory and generate the projet makefiles in it.

```
$ mkdir build
$ cd build
$ cmake /path/to/ssf/source -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=
```

Various parameters can be customized when generating the project files:

* `CMAKE_BUILD_TYPE`: `Debug`, `RelWithDebInfo`, `Release` or `MinSizeRel`. Build type.
* `CMAKE_INSTALL_PREFIX`: Install directory prefix.
* `USE_STATIC_LIBS`: `ON` or `OFF` to enable/disable linking statically against
boost and OpenSSL. It is recommended to set this to `ON` if you intend to build
and run SSF on different environments. The default is `OFF`.
* `USE_STATIC_RUNTIME`: `ON` or `OFF` to enable/disable linking statically
against libstdc++. This is set automatically to the same value as
`USE_STATIC_LIBS`.
* `BUILD_UNIT_TESTS`: `ON` or `OFF` to enable/disable building SSF unit tests.
* `DISABLE_RTTI`: `ON` or `OFF` to disable/enable C++ Run-Time Type Information.
RTTI is enabled by default. SSF does not currently build on Unix without RTTI.
* `DISABLE_TLS`: `ON` or `OFF` to disable/enable TLS layer. Network traffic will
use raw TCP and be left unsecured. Provided for testing purpose only.

Proceed to build SSF:

```
$ make
```

You can install SSF on your system using `make install`. The full install
directory is the content of `CMAKE_INSTALL_PREFIX` prepended to `DESTDIR`.

```
$ make install DESTDIR=/install_path
```