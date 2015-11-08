Linux Kernel Build Guide (For WiTi Board)
----
If you want build & upgrade your kernel only. Here is the tutorial shows you how to do it.

Get Kernel Source
----
First, get the kernel source code to compile

    git clone --depth=1 https://github.com/mqmaker/linux.git


Get Toolchain
----
Next, you need a cross compiler to compile the kernel for MIPS platform. You can get the toolchain from our repository.

    git clone --depth=1 -b mips-2012.03 https://github.com/mqmaker/toolchain.git
  
You might put the toolchain to `/opt` or any location you like.

Start compile
----
* Grab a valid kernel config from our repo

```shell
  cd linux
  curl https://raw.githubusercontent.com/mqmaker/witi-config/master/witi_kernel_config_default > .config
```
* You might want to customize your kernel

```shell
export PATH=/opt/mips-2012.03/bin/:$PATH
make ARCH="mips" menuconfig
```

* Compile the kernel
```shell
export PATH=/opt/mips-2012.03/bin/:$PATH

rm -vf uImage*
rm -vf vmlinux*

make \
-j8 \
CROSS_COMPILE="mipsel-linux-" \
ARCH="mips" \
CC="mipsel-linux-gcc -D__linux__" \
uImage

mipsel-linux-objcopy \
-O binary \
-R .reginfo \
-R .notes \
-R .note \
-R .comment \
-R .mdebug \
-R .note.gnu.build-id \
-S vmlinux vmlinux

lzma vmlinux

mkimage \
-A mips \
-O linux \
-T kernel \
-C lzma \
-a 0x80001000 \
-e 0x80001000 \
-n "Linux-3.10.14" \
-d vmlinux.lzma uImage
```
If everything goes fine, then your new kernel names `uImage` is ready to use.
