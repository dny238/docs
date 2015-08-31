Preparation
------
You need to install Linux OS. We suggest to install Ubuntu 14.04. And ensure that
you have installed follows software.

      sudo apt-get install g++ libncurses5-dev zlib1g-dev bison flex unzip autoconf gawk make \
      gettext gcc binutils patch bzip2 libz-dev asciidoc subversion git

Get source code
------
      git clone --depth=1 https://github.com/mqmaker/witi-openwrt.git

Configure
------
* First, select correct target profile. As follows:

            $ make menuconfig
            
            #Select Target system
            Target System (MTK/Ralink APSOC) --->
            Subtarget (MT7621 based boards) --->
                  (   ) MT7620 based boards
                  ( x ) MT7621 based boards
                  (   ) MT7628 based boards
            
            #Select Target Profile
            Target Profile (MQmaker Witi Board Profile) --->
                  (   ) Default Profile
                  ( x ) MQmaker Witi Board Profile
            
            #Select Advanced configuration options (for developers) --->
            #Select Toolchain Options  --->
            #Select Binutils Verion (binutils 2.22) --->
                  (   ) ...
                  ( x ) Linaro bintutils 2.24
            #Select Gcc compiler Version --->
                  (   ) ...
                  ( x ) gcc 5.2.0
            #Select C Library implementation --->
                  ( x ) Use eglibc
                  (   ) Use uClibc
            #Check [*] Build/install gccgo compiler?
* Compile<br>
Now, we have completed minimum configuration. Next you can compile WiTi source code.
If you want to show the process of compilationk, user V=s.

            make V=s -j 4
or

            make -j 4
OK,  enjoy a cup of tea, wait it complete. ^_^

Using gccgo cross compiler
------
* Set enviroment variable

            cd staging_dir (In your clone of witi-openwrt repo)
            export STAGING_DIR=$(pwd)
            
* At anywhere, create a `helloworld.go`

      ```go
      package main
      import "fmt"
      
      func main() {
      	fmt.Println("Hello, World")
      }
      ```
* Compile your go program

            path/to/mipsel-openwrt-linux-gccgo helloworld.go -o hello -static-libgo


Now we done, Have fun!
