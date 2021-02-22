+++
title = "Run RISCV VM Test Suite"
author = ["Aimee Z"]
description = "Nervos CKB-VM, a RISCV VM."
date = 2021-02-20
tags = ["riscv", "vm", "ckb"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2005
  identifier = "run-riscv-vm-test-suite"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Install RISCV toolchain](#install-riscv-toolchain)
- [Run ckb-vm-test-suite](#run-ckb-vm-test-suite)
- [References](#references)

</div>
<!--endtoc-->

RISCV:

-   <https://github.com/riscv/riscv-gnu-toolchain>
-   <https://github.com/riscv/riscv-bitmanip/blob/master/bitmanip-draft.pdf>

CKB-VM:

-   <https://github.com/nervosnetwork/ckb-vm>
-   <https://github.com/nervosnetwork/ckb-vm-test-suite>


## Install RISCV toolchain {#install-riscv-toolchain}

I spent a long time installing [RISCV GNU Toolchain](https://github.com/riscv/riscv-gnu-toolchain).
Its instructions for macOS do not seem right, and I am not familiar with system-level programming.

Follow the instructions from its GitHub.

```shell
$ git clone https://github.com/riscv/riscv-gnu-toolchain
Cloning into 'riscv-gnu-toolchain'...
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 7352 (delta 4), reused 9 (delta 2), pack-reused 7336
Receiving objects: 100% (7352/7352), 4.50 MiB | 2.58 MiB/s, done.
Resolving deltas: 100% (3591/3591), done.
warning: the following paths have collided (e.g. case-sensitive paths
on a case-insensitive filesystem) and only one from the same
colliding group is in the working tree:

  'linux-headers/include/linux/netfilter/xt_CONNMARK.h'
  'linux-headers/include/linux/netfilter/xt_connmark.h'
  'linux-headers/include/linux/netfilter/xt_DSCP.h'
  'linux-headers/include/linux/netfilter/xt_dscp.h'
  'linux-headers/include/linux/netfilter/xt_MARK.h'
  'linux-headers/include/linux/netfilter/xt_mark.h'
  'linux-headers/include/linux/netfilter/xt_RATEEST.h'
  'linux-headers/include/linux/netfilter/xt_rateest.h'
  'linux-headers/include/linux/netfilter/xt_TCPMSS.h'
  'linux-headers/include/linux/netfilter/xt_tcpmss.h'
  'linux-headers/include/linux/netfilter_ipv4/ipt_ECN.h'
  'linux-headers/include/linux/netfilter_ipv4/ipt_ecn.h'
  'linux-headers/include/linux/netfilter_ipv4/ipt_TTL.h'
  'linux-headers/include/linux/netfilter_ipv4/ipt_ttl.h'
  'linux-headers/include/linux/netfilter_ipv6/ip6t_HL.h'
  'linux-headers/include/linux/netfilter_ipv6/ip6t_hl.h'
```

Meanwhile, in another window:

```shell
$ brew install python3 gawk gnu-sed gmp mpfr libmpc isl zlib expat
```

Go back to previous window:

```shell
$ cd riscv-gnu-toolchain/
$ ./configure --prefix=/opt/riscv
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for grep that handles long lines and -e... /usr/bin/grep
checking for fgrep... /usr/bin/grep -F
checking for grep that handles long lines and -e... (cached) /usr/bin/grep
checking for bash... /bin/sh
checking for __gmpz_init in -lgmp... yes
checking for mpfr_init in -lmpfr... yes
checking for mpc_init2 in -lmpc... yes
checking for curl... /usr/bin/curl
checking for wget... /usr/local/bin/wget
checking for ftp... no
configure: creating ./config.status
config.status: creating Makefile
config.status: creating scripts/wrapper/awk/awk
config.status: creating scripts/wrapper/sed/sed
```

`make`:

```shell
$ make
cd /<my_path>/riscv-gnu-toolchain && \
      flock /<my_path>/riscv-gnu-toolchain/.git/config git submodule init /<my_path>/riscv-gnu-toolchain/riscv-gcc/ && \
      flock /<my_path>/riscv-gnu-toolchain/.git/config git submodule update /<my_path>/riscv-gnu-toolchain/riscv-gcc/
/bin/sh: flock: command not found
make: *** [/<my_path>/riscv-gnu-toolchain/riscv-gcc/.git] Error 127
```

After a bit of searching, I was told that
because of different filesystems, I need to
create another workspace with the file system that is needed by
RISCV toolchain:

```shell
$ hdiutil create -type SPARSE -fs 'Case-sensitive Journaled APFS' -size 60g -volname workspace riscvfs
hdiutil: create failed - Invalid argument

$ df -h
Filesystem       Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk1s1s1  466Gi   14Gi  155Gi     9%  567557 4881885323    0%   /
devfs           192Ki  192Ki    0Bi   100%     664          0  100%   /dev
/dev/disk1s5    466Gi  6.0Gi  155Gi     4%       6 4882452874    0%   /System/Volumes/VM
/dev/disk1s3    466Gi  344Mi  155Gi     1%    1170 4882451710    0%   /System/Volumes/Preboot
/dev/disk1s6    466Gi  588Ki  155Gi     1%      15 4882452865    0%   /System/Volumes/Update
/dev/disk1s2    466Gi  290Gi  155Gi    66% 3101937 4879350943    0%   /System/Volumes/Data
map auto_home     0Bi    0Bi    0Bi   100%       0          0  100%   /System/Volumes/Data/home

$ mount
/dev/disk1s1s1 on / (apfs, sealed, local, read-only, journaled)
devfs on /dev (devfs, local, nobrowse)
/dev/disk1s5 on /System/Volumes/VM (apfs, local, noexec, journaled, noatime, nobrowse)
/dev/disk1s3 on /System/Volumes/Preboot (apfs, local, journaled, nobrowse)
/dev/disk1s6 on /System/Volumes/Update (apfs, local, journaled, nobrowse)
/dev/disk1s2 on /System/Volumes/Data (apfs, local, journaled, nobrowse)
map auto_home on /System/Volumes/Data/home (autofs, automounted, nobrowse)

$ hdiutil create -type SPARSE -fs 'Case-sensitive APFS' -size 60g -volname workspace riscvfs
created: /<my_path>/riscvfs.sparseimage

$ hdiutil attach riscvfs.sparseimage
/dev/disk2          	GUID_partition_scheme
/dev/disk2s1        	EFI
/dev/disk2s2        	Apple_APFS
/dev/disk3          	EF57347C-0000-11AA-AA11-0030654
/dev/disk3s1        	41504653-0000-11AA-AA11-0030654	/Volumes/workspace
```

In the workspace, clone the toolchain:

```shell
$ cd /Volumes/workspace/
$ ls
$ git clone https://github.com/riscv/riscv-gnu-toolchain
Cloning into 'riscv-gnu-toolchain'...
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 7352 (delta 4), reused 9 (delta 2), pack-reused 7336
Receiving objects: 100% (7352/7352), 4.50 MiB | 2.28 MiB/s, done.
Resolving deltas: 100% (3591/3591), done.
```

Configure:

```shell
$ cd riscv-gnu-toolchain/
$ ./configure --prefix=/opt/riscv

checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for grep that handles long lines and -e... /usr/bin/grep
checking for fgrep... /usr/bin/grep -F
checking for grep that handles long lines and -e... (cached) /usr/bin/grep
checking for bash... /bin/sh
checking for __gmpz_init in -lgmp... yes
checking for mpfr_init in -lmpfr... yes
checking for mpc_init2 in -lmpc... yes
checking for curl... /usr/bin/curl
checking for wget... /usr/local/bin/wget
checking for ftp... no
configure: creating ./config.status
config.status: creating Makefile
config.status: creating scripts/wrapper/awk/awk
config.status: creating scripts/wrapper/sed/sed
```

PATH:

```shell
$ export PATH=$PATH:/opt/riscv/bin
```

`make` still doesn't work:

```shell
$ make
cd /Volumes/workspace/riscv-gnu-toolchain && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule init /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/ && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule update /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/
/bin/sh: flock: command not found
make: *** [/Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/.git] Error 127
```

Check `flock`:

```shell
$ brew info flock
flock: 2.2.472
https://flock.com/
Not installed
From: https://github.com/Homebrew/homebrew-cask/blob/HEAD/Casks/flock.rb
==> Name
Flock
==> Description
Business messaging and team collaboration app
==> Artifacts
Flock.app (App)
==> Analytics
install: 77 (30 days), 219 (90 days), 577 (365 days)
```

Keep trying `make`:

```shell
$ make
cd /Volumes/workspace/riscv-gnu-toolchain && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule init /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/ && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule update /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/
/bin/sh: flock: command not found
make: *** [/Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/.git] Error 127

$ make linux
cd /Volumes/workspace/riscv-gnu-toolchain && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule init /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/ && \
      flock /Volumes/workspace/riscv-gnu-toolchain/.git/config git submodule update /Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/
/bin/sh: flock: command not found
make: *** [/Volumes/workspace/riscv-gnu-toolchain/riscv-gcc/.git] Error 127
```

Change the configure and disable Linux:

```shell
$ ./configure --prefix=/opt/riscv --disable-linux --disable-multilib
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for grep that handles long lines and -e... /usr/bin/grep
checking for fgrep... /usr/bin/grep -F
checking for grep that handles long lines and -e... (cached) /usr/bin/grep
checking for bash... /bin/sh
checking for __gmpz_init in -lgmp... yes
checking for mpfr_init in -lmpfr... yes
checking for mpc_init2 in -lmpc... yes
checking for curl... /usr/bin/curl
checking for wget... /usr/local/bin/wget
checking for ftp... no
configure: creating ./config.status
config.status: creating Makefile
config.status: creating scripts/wrapper/awk/awk
config.status: creating scripts/wrapper/sed/sed
```

Follow the instructions for the `submodule`:

```shell
$ git submodule update --init --recursive
```

And then `make`:

```shell
$ make
mkdir -p /opt/riscv/.test || \
              (echo "Sorry, you don't have permission to write to" \
               "'/opt/riscv', use --prefix to specify" \
               "another path, or use 'sudo make' if you *REALLY* want to" \
               "install into '/opt/riscv'" && exit 1)
mkdir: /opt/riscv/.test: Permission denied
Sorry, you don't have permission to write to '/opt/riscv', use --prefix to specify another path, or use 'sudo make' if you *REALLY* want to install into '/opt/riscv'
make: *** [stamps/check-write-permission] Error 1

$ sudo make
Password:
mkdir -p /opt/riscv/.test || \
              (echo "Sorry, you don't have permission to write to" \
               "'/opt/riscv', use --prefix to specify" \
               "another path, or use 'sudo make' if you *REALLY* want to" \
               "install into '/opt/riscv'" && exit 1)
rm -r /opt/riscv/.test
mkdir -p stamps/ && touch stamps/check-write-permission
rm -rf stamps/build-binutils-newlib build-binutils-newlib
mkdir build-binutils-newlib
cd build-binutils-newlib && CC_FOR_TARGET=riscv64-unknown-elf-gcc /Volumes/workspace/riscv-gnu-toolchain/riscv-binutils/configure \
              --target=riscv64-unknown-elf \
               \
              --prefix=/opt/riscv \
               \
              --disable-werror \
              --with-expat=yes  \
              --disable-gdb \
              --disable-sim \
              --disable-libdecnumber \
              --disable-readline
checking build system type... x86_64-apple-darwin20.2.0
checking host system type... x86_64-apple-darwin20.2.0
checking target system type... riscv64-unknown-elf
checking for a BSD-compatible install... /usr/bin/install -c
checking whether ln works... yes
...
```

Check the installing results:

```shell
$ ls
Applications	System		Volumes		cores		etc		opt		sbin		usr
Library		Users		bin		dev		home		private		tmp		var
$ ls opt/
riscv
$ ls opt/riscv/
bin			include			lib			libexec			riscv64-unknown-elf	share
$ ls Volumes/
Macintosh HD	workspace
$ ls Volumes/workspace/
riscv-gnu-toolchain
```


## Run ckb-vm-test-suite {#run-ckb-vm-test-suite}

Fetch GitHub repo:

```shell
# inside ckb-vm-test-suite
ckb-vm-test-suite aimeez$ cd ckb-vm
$ git fetch mohanson
...
$ git checkout mohanson/bextension
```

Run test:

```shell
$ ./test.sh
...
/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/vm.c: In function 'coherence_torture':
<command-line>: error: invalid suffix "x" on integer constant
/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/vm.c:213:25: note: in expansion of macro 'ENTROPY'
  213 |   unsigned int random = ENTROPY;
      |                         ^~~~~~~
/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/vm.c: In function 'vm_boot':
<command-line>: error: invalid suffix "x" on integer constant
/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/vm.c:228:25: note: in expansion of macro 'ENTROPY'
  228 |   unsigned int random = ENTROPY;
      |                         ^~~~~~~
make[1]: *** [rv32ui-v-simple] Error 1
make: *** [isa] Error 2
```

Regex `ENTROPY`:

```shell
$ cd riscv-tests/
$ rg ENTROPY
isa/Makefile
61:	$$(RISCV_GCC) $(2) $$(RISCV_GCC_OPTS) -DENTROPY=0x$$(shell echo \$$@ | md5sum | cut -c 1-7) -std=gnu99 -O2 -I$(src_dir)/../env/v -I$(src_dir)/macros/scalar -T$(src_dir)/../env/v/link.ld $(src_dir)/../env/v/entry.S $(src_dir)/../env/v/*.c $$< -o $$@
65:	$$(RISCV_GCC) $(2) $$(RISCV_GCC_OPTS) -DENTROPY=0x$$(shell echo \$$@ | md5sum | cut -c 1-7) -std=gnu99 -O2 -I$(src_dir)/../env/u -I$(src_dir)/macros/scalar -T$(src_dir)/../env/u/link.ld $(src_dir)/../env/u/entry.S $(src_dir)/../env/u/*.c $$< -o $$@

env/v/vm.c
213:  unsigned int random = ENTROPY;
228:  unsigned int random = ENTROPY;
```

Install `md5sum` with Homebrew:

```shell
$ brew install md5sha1sum
```

Run test:

```shell
$ ./test.sh
...
+ make isa
mkdir -p isa
/Applications/Xcode.app/Contents/Developer/usr/bin/make -C isa -f /<my_path>/ckb-vm-test-suite/riscv-tests/isa/Makefile src_dir=/<my_path>/ckb-vm-test-suite/riscv-tests/isa XLEN=64
riscv64-unknown-elf-gcc -march=rv32g -mabi=ilp32 -static -mcmodel=medany -fvisibility=hidden -nostdlib -nostartfiles -DENTROPY=0xf7930f7 -std=gnu99 -O2 -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/macros/scalar -T/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/link.ld /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/entry.S /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/*.c rv32ui/simple.S -o rv32ui-v-simple
dyld: Library not loaded: /usr/local/opt/isl/lib/libisl.22.dylib
  Referenced from: /opt/riscv/libexec/gcc/riscv64-unknown-elf/10.2.0/cc1
  Reason: image not found
riscv64-unknown-elf-gcc: internal compiler error: Abort trap: 6 signal terminated program cc1
Please submit a full bug report,
with preprocessed source if appropriate.
See <https://gcc.gnu.org/bugs/> for instructions.
make[1]: *** [rv32ui-v-simple] Error 4
make: *** [isa] Error 2
```

Try installing `dylib` and `isl`:

```shell
$ brew install dylib
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
Updated 4 formulae.

==> Searching for similarly named formulae...
This similarly named formula was found:
dylibbundler
To install it, run:
  brew install dylibbundler
Error: No available formula or cask with the name "dylib".
==> Searching for a previously deleted formula (in the last month)...
Error: No previously deleted formula found.
==> Searching taps on GitHub...
Error: No formulae found in taps.

$ brew install isl
Warning: isl 0.23 is already installed and up-to-date.
To reinstall 0.23, run:
  brew reinstall isl

$ brew reinstall isl
==> Downloading https://homebrew.bintray.com/bottles/isl-0.23.big_sur.bottle.tar.gz
Already downloaded: /<my_path>/Homebrew/downloads/38d200e8ed4652dc3b9e6a7ed6b341d18d8756fde1f7575c8a437a0436a55ae6--isl-0.23.big_sur.bottle.tar.gz
==> Reinstalling isl
==> Pouring isl-0.23.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/isl/0.23: 72 files, 5MB
Removing: /usr/local/Cellar/isl/0.22.1... (72 files, 4.7MB)
```

Run test:

```shell
$ ./test.sh
...
+ make isa
mkdir -p isa
/Applications/Xcode.app/Contents/Developer/usr/bin/make -C isa -f /<my_path>/ckb-vm-test-suite/riscv-tests/isa/Makefile src_dir=/<my_path>/ckb-vm-test-suite/riscv-tests/isa XLEN=64
riscv64-unknown-elf-gcc -march=rv32g -mabi=ilp32 -static -mcmodel=medany -fvisibility=hidden -nostdlib -nostartfiles -DENTROPY=0xf7930f7 -std=gnu99 -O2 -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/macros/scalar -T/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/link.ld /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/entry.S /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/*.c rv32ui/simple.S -o rv32ui-v-simple
dyld: Library not loaded: /usr/local/opt/isl/lib/libisl.22.dylib
  Referenced from: /opt/riscv/libexec/gcc/riscv64-unknown-elf/10.2.0/cc1
  Reason: image not found
riscv64-unknown-elf-gcc: internal compiler error: Abort trap: 6 signal terminated program cc1
Please submit a full bug report,
with preprocessed source if appropriate.
See <https://gcc.gnu.org/bugs/> for instructions.
make[1]: *** [rv32ui-v-simple] Error 4
make: *** [isa] Error 2
```

Check gcc:

```shell
$ gcc -v
Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 12.0.0 (clang-1200.0.32.29)
Target: x86_64-apple-darwin20.2.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

Check `isl` and find out my version is 0.23
while the test suite uses 0.22:

```shell
$ brew search isl
==> Formulae
dislocker                               isl ✔                                   isl@0.18                                redis-leveldb
==> Casks
islide

$ brew info isl
isl: stable 0.23 (bottled), HEAD
Integer Set Library for the polyhedral model
http://isl.gforge.inria.fr
/usr/local/Cellar/isl/0.23 (72 files, 5MB) *
  Poured from bottle on 2021-02-19 at 16:19:35
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/isl.rb
License: MIT
==> Dependencies
Required: gmp ✔
==> Options
--HEAD
      Install HEAD version
==> Analytics
install: 101,561 (30 days), 262,122 (90 days), 910,365 (365 days)
install-on-request: 3,059 (30 days), 7,888 (90 days), 19,041 (365 days)
build-error: 0 (30 days)
```

Try installing isl 0.22:

```shell
$ brew install isl@0.22
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
Updated 3 formulae.

==> Searching for similarly named formulae...
Error: No similarly named formulae found.
Error: No available formula or cask with the name "isl@0.22".
==> Searching for a previously deleted formula (in the last month)...
Error: No previously deleted formula found.
==> Searching taps on GitHub...
Error: No formulae found in taps.
```

Check my files:

```shell
$ cd /opt/riscv/
$ ls
bin			lib			riscv64-unknown-elf
include			libexec			share

$ otool -L libexec/gcc/riscv64-unknown-elf/10.2.0/cc1
libexec/gcc/riscv64-unknown-elf/10.2.0/cc1:
      /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
      /usr/local/opt/isl/lib/libisl.22.dylib (compatibility version 23.0.0, current version 23.1.0)
      /usr/local/opt/libmpc/lib/libmpc.3.dylib (compatibility version 6.0.0, current version 6.1.0)
      /usr/local/opt/mpfr/lib/libmpfr.6.dylib (compatibility version 8.0.0, current version 8.0.0)
      /usr/local/opt/gmp/lib/libgmp.10.dylib (compatibility version 15.0.0, current version 15.1.0)
      /usr/lib/libz.1.dylib (compatibility version 1.0.0, current version 1.2.11)
      /usr/local/opt/zstd/lib/libzstd.1.dylib (compatibility version 1.0.0, current version 1.4.8)
      /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 904.4.0)
      /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1292.60.1)

# link 0.23 to 0.22
$ ln -s /usr/local/opt/isl/lib/libisl.23.dylib  /usr/local/opt/isl/lib/libisl.22.dylib
$ ls -l /usr/local/opt/isl/lib/
total 8392
lrwxr-xr-x  1 aimeez  staff       38 Feb 19 16:46 libisl.22.dylib -> /usr/local/opt/isl/lib/libisl.23.dylib
-rw-r--r--  1 aimeez  staff  1653040 Feb 19 16:19 libisl.23.dylib
-r--r--r--  1 aimeez  staff  2641144 Nov 11 09:18 libisl.a
lrwxr-xr-x  1 aimeez  staff       15 Nov 11 09:18 libisl.dylib -> libisl.23.dylib
drwxr-xr-x  3 aimeez  staff       96 Feb 19 16:19 pkgconfig
```

Run test:

```shell
$ ./test.sh
...
config.status: creating Makefile
+ make isa
mkdir -p isa
/Applications/Xcode.app/Contents/Developer/usr/bin/make -C isa -f /<my_path>/ckb-vm-test-suite/riscv-tests/isa/Makefile src_dir=/<my_path>/ckb-vm-test-suite/riscv-tests/isa XLEN=64
riscv64-unknown-elf-gcc -march=rv32g -mabi=ilp32 -static -mcmodel=medany -fvisibility=hidden -nostdlib -nostartfiles -DENTROPY=0xf7930f7 -std=gnu99 -O2 -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v -I/<my_path>/ckb-vm-test-suite/riscv-tests/isa/macros/scalar -T/<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/link.ld /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/entry.S /<my_path>/ckb-vm-test-suite/riscv-tests/isa/../env/v/*.c rv32ui/simple.S -o rv32ui-v-simple
/opt/riscv/lib/gcc/riscv64-unknown-elf/10.2.0/../../../../riscv64-unknown-elf/bin/ld: /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T//ccMdcSBH.o: in function `tohost':
(.tohost+0x0): multiple definition of `tohost'; /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T//ccNt0arm.o:(.sbss+0x10): first defined here
/opt/riscv/lib/gcc/riscv64-unknown-elf/10.2.0/../../../../riscv64-unknown-elf/bin/ld: /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T//ccMdcSBH.o: in function `fromhost':
(.tohost+0x40): multiple definition of `fromhost'; /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T//ccNt0arm.o:(.sbss+0x8): first defined here
collect2: error: ld returned 1 exit status
make[1]: *** [rv32ui-v-simple] Error 1
make: *** [isa] Error 2
```

According to RISCV's issue:
<https://github.com/riscv/riscv-tools/issues/316#issuecomment-667659789>

Fix it by changing C code:
ckb-vm-test-suite/riscv-tests/env/v/vm.c

```c
extern volatile uint64_t tohost;
extern volatile uint64_t fromhost;
```

Rust test:

```shell
+ make
cd deps/secp256k1 && \
              ./autogen.sh && \
              CC=riscv64-unknown-elf-gcc LD=riscv64-unknown-elf-gcc ./configure --with-bignum=no --enable-ecmult-static-precomputation --enable-endomorphism --host=riscv64-elf && \
              make src/ecmult_static_pre_context.h src/ecmult_static_context.h
Can't exec "aclocal": No such file or directory at /usr/local/Cellar/autoconf/2.69/share/autoconf/Autom4te/FileUtils.pm line 326.
autoreconf: failed to run aclocal: No such file or directory
make: *** [deps/secp256k1/src/ecmult_static_pre_context.h] Error 1
```

Install `automake` to auto-generating makefiles:

```shell
$ brew install automake
```

All tests are passed.

```shell
+ echo 'All tests are passed!'
All tests are passed!
```


## References {#references}

-   [dixson3/workspace.sh](https://gist.github.com/dixson3/8360571)
-   [scottsb/casesafe.sh](https://gist.github.com/scottsb/479bebe8b4b86bf17e2d)
-   [RISCV-tools issue](https://github.com/riscv/riscv-tools/issues/316#issuecomment-667659789)