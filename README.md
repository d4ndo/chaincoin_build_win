Cross compile Chaincoin
=======================

Host: Ubuntu

Target: Windows 7/8/10
64 Bit

Toolchain mxe
=============


Install
-------

Clone the crosscompiler mxe toolchain


```bash
git clone https://github.com/mxe/mxe.git

```

Settings
--------

To compile for 64 bit edit ./settings.mk. Add the following entry.  

```bash

MXE_TARGETS := x86_64-w64-mingw32.static

```

change Berkeley DB to version 4.8


```bash
cd src
cp db.mk db4.8.mk
vim db4.8.mk
PKG             := db4.8
$(PKG)_VERSION  := 4.8.30
$(PKG)_CHECKSUM := e0491a07cdb21fb9aa82773bbbedaeb7639cbd0e7f96147ab46141e0045db72a
```

Compile libraries
-----------------

The following libraries are needed to compile chaincoin:


```bash
cd mxe
make openssl libevent boost miniupnpc qt db4.8
```

Enviornment
-----------

```bash
export PATH=$HOME/mxe/usr/bin:$PATH

unset `env | \
    grep -vi '^EDITOR=\|^HOME=\|^LANG=\|MXE\|^PATH=' | \
    grep -vi 'PKG_CONFIG\|PROXY\|^PS1=\|^TERM=' | \
    cut -d '=' -f1 | tr '\n' ' '`
```

Compile CHAINCOIN
=================

```bash
git clone https://github.com/chaincoin/chaincoin.git
cd chaincoin

./configure --host=x86_64-w64-mingw32.static --enable-static --disable-shared --disable-tests --with-boost=$HOME/mxe/usr/x86_64-w64-mingw32.static/include/boost --with-boost-libdir=$HOME/mxe/usr/x86_64-w64-mingw32.static/lib

make HOST=x86_64-w64-mingw32 -j4
```

