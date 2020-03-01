# Duktape Windows

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/mengmo/Duktape-Windows?logo=github)](https://github.com/mengmo/Duktape-Windows/releases)

[![GitHub issues](https://img.shields.io/github/issues/mengmo/Duktape-Windows?logo=github)](https://github.com/mengmo/Duktape-Windows/issues)
[![GitHub stars](https://img.shields.io/github/stars/mengmo/Duktape-Windows?logo=github)](https://github.com/mengmo/Duktape-Windows/stargazers)
[![GitHub All Releases](https://img.shields.io/github/downloads/mengmo/Duktape-Windows/total?logo=github)](https://github.com/mengmo/Duktape-Windows/releases)
[![GitHub license](https://img.shields.io/github/license/mengmo/Duktape-Windows?logo=open-source-initiative)](https://github.com/mengmo/Duktape-Windows/blob/master/LICENSE)

Method to build [Duktape](https://duktape.org/) on Windows, and [prebuilt binary releases](https://github.com/mengmo/Duktape-Windows/releases).

## Build Prerequisites
Install [MSYS2](http://www.msys2.org/)

If build 64-bit Duktape with `MINGW64`, install `x86_64-toolchain`
```
pacman -S mingw-w64-x86_64-toolchain
echo "#! /bin/sh" > /mingw64/bin/make
echo "\"mingw32-make\" \"\$@\"" >> /mingw64/bin/make
```

If build 32-bit Duktape with `MINGW32`, install `i686-toolchain`
```
pacman -S mingw-w64-i686-toolchain
echo "#! /bin/sh" > /mingw32/bin/make
echo "\"mingw32-make\" \"\$@\"" >> /mingw32/bin/make
```

Get [Duktape](https://github.com/svaarala/duktape) latest release from [![GitHub release (latest by date)](https://img.shields.io/github/v/release/svaarala/duktape?logo=github&style=plastic)](https://github.com/svaarala/duktape/releases)

## Compilation
```
tar -xvJf duktape-2.5.0.tar.xz
cd ./duktape-2.5.0
make -f Makefile.cmdline DEFINES="-Wl,-static,-s"
```
Here is enough if you'd like to test only

## Build Windows DLL
### Prerequisites
If build 64-bit `dll` with `MINGW64`, install `mingw-w64-x86_64-python2-yaml` in `MSYS2` without `MINGW64` running.
```
pacman -S mingw-w64-x86_64-python2-yaml
```

If build 32-bit `dll` with `MINGW32`, install `mingw-w64-i686-python2-yaml` in `MSYS2` without `MINGW32` running.
```
pacman -S mingw-w64-i686-python2-yaml
```
### Compilation
```
python2 tools/configure.py --output-directory ./libduktape --dll
gcc -shared -o libduktape.dll -Wl,-static,-s -fPIC -Os -pedantic -std=c99 -Wall -Wextra -fstrict-aliasing -fomit-frame-pointer -I./libduktape ./libduktape/duktape.c -lm
```
### Build static library
```
gcc -c ./libduktape/duktape.c
ar rcs libduktape.a duktape.o
```

## Packaging
```
mkdir ./bin
mv duk.exe ./bin
mkdir ./lib
mv libduktape.dll libduktape.a ./lib
mkdir ./include
cp -p ./libduktape/duk_config.h ./libduktape/duktape.h ./include
zip -9 -r $(basename `pwd`)-win$(echo ${MSYSTEM:0-2}).zip ./bin ./include ./lib LICENSE.txt README.rst
```
