esp-open-sdk
-------------

Ecohub 용으로 사용하기 위해 몇가지 수정 및 모듈 추가

추가된 서브 모듈
- esptool2
- Sming

Mac OSX 에서만 테스트 완료

## MacOS:
```bash
$ brew tap homebrew/dupes
$ brew install binutils coreutils automake wget gawk libtool help2man gperf gnu-sed --with-default-names grep
```

In addition to the development tools MacOS needs a case-sensitive filesystem.
You might need to create a virtual disk and build esp-open-sdk on it:
```bash
$ sudo hdiutil create ~/Documents/case-sensitive.dmg -volname "case-sensitive" -size 10g -fs "Case-sensitive HFS+"
$ sudo hdiutil mount ~/Documents/case-sensitive.dmg
$ cd /Volumes/case-sensitive
```

Building
========

Be sure to clone recursively:

```
$ git clone --recursive git@github.com:hanoseok/esp-open-sdk.git 
```

The project can be built in two modes:

1. Where the toolchain and tools are kept separate from the vendor IoT SDK
   which contains binary blobs. This makes licensing more clear, and helps
   facilitate upgrades to vendor SDK releases.

2. A completely standalone ESP8266 SDK with the vendor SDK files merged
   into the toolchain. This mode makes it easier to build software (no
   additinal `-I` and `-L` flags are needed), but redistributability of
   this build is unclear and upgrades to newer vendor IoT SDK releases are
   complicated. This mode is default for local builds. Note that if you
   want to redistribute the binary toolchain built with this mode, you
   should:

    1. Make it clear to your users that the release is bound to a
       particular vendor IoT SDK and provide instructions how to upgrade
       to a newer vendor IoT SDK releases.
    2. Abide by licensing terms of the vendor IoT SDK.

To build the self-contained, standalone toolchain+SDK:

```
$ make STANDALONE=y
```

This is the default choice which most people are looking for, so just the
following is enough:

```
$ make
```

To build the bare Xtensa toolchain and leave ESP8266 SDK separate:

```
$ make STANDALONE=n
```

This will download all necessary components and compile them.

