Preparation
------
You need to install Linux OS. We suggest to install Ubuntu 14.04. And ensure that
you have installed follows software.

      sudo apt-get install g++ libncurses5-dev zlib1g-dev bison flex unzip autoconf gawk make \
      gettext gcc binutils patch bzip2 libz-dev asciidoc subversion git

Get source code
------
      git clone --depth=1 https://github.com/mqmaker/witi-openwrt.git
      git clone --depth=1 https://github.com/mqmaker/witi-uboot.git

Configure & Compile to WiTi
------
When you download the source code of witi-openwrt, you should configure to compile openwrt. 
Let's begin!

* Update & install software packages<br> 
We should update some packages form other repositories, and install it.

            ./scripts/feeds update -a
            ./scripts/feeds install -a

* Configure<br>
First, select correct target profile. As follows:

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
            
            #Select the type of Images
            Target Images  --->
                  [ * ] ramdisk  --->
                  .....
                  [ * ] squashfs --->

* Compile<br>
Now, we have completed minimum configuration. Next you can compile WiTi source code.
If you want to show the process of compilationk, user V=s.

            make V=s
or

            make
Ok,  enjoy a cup of tea, wait it complete. ^_^

Download image to WiTi Board
------
Until then, it will generate an image for WiTi Board at "bin/ramips/" directory. 
Find it, and copy it to your tftp server directory.For example:

            $ cd bin/ramips/
            $ ls
            $ openwrt-ramips-mt7621-witi-squashfs-sysupgrade.bin   openwrt-ramips-mt7621-witi-initramfs-uImage.bin
            $ cp openwrt-ramips-mt7621-witi-squashfs-sysupgrade.bin /tftpboot/ -vfr
Before download imge to the WiTi Board, you should configure your serial parameters.
If not, minicom can not receive log correctly. As that:

            Speed:   57600
            Parity:   None
            Data:     8
            Stopbits: 1
Then, reboot your WiTi Board. When it run to here:

            U-Boot 1.1.3 (Apr  7 2015 - 20:32:44)
            
            Board: Ralink APSoC DRAM:  256 MB
            relocate_code Pointer at: 8ffb8000
            
            ......
            
            ##### The CPU freq = 880 MHZ #### 
            estimate memory size =256 Mbytes
            #Reset_MT7530
            
            Please choose the operation: 
            1: Load system code to SDRAM via TFTP. 
            2: Load system code then write to Flash via TFTP. 
            3: Boot system code via Flash (default).
            4: Entr boot command line interface.
            7: Load Boot Loader code then write to Flash via Serial. 
            9: Load Boot Loader code then write to Flash via TFTP. 
            1
Select 2 to Flash image at first time. Then select Y, enter ip address, 
the filename of image, and press ENTER.

            You choosed 2
            2: System Load Linux Kernel then write to Flash via TFTP. 
            Warning!! Erase Linux in Flash then burn new one. Are you sure?(Y/N)
            Y
            
             Please Input new ones /or Ctrl-C to discard
             Input device IP (10.10.10.123) ==:10.10.10.123
             Input server IP (10.10.10.3) ==:10.10.10.3
             Input Linux Kernel filename () ==: openwrt-ramips-mt7621-witi-squashfs-sysupgrade.bin
             
            ......
            
            Filename 'openwrt-ramips-mt7621-witi-squashfs-sysupgrade.bin'.
            TIMEOUT_COUNT=10,Load address: 0x80100000
            Loading: checksum bad
            Got ARP REPLY, set server/gtwy eth addr (d8:cb:8a:13:7e:35)
            Got it
            #################################################################
               #################################################################
               #################################################################
            ......

Enjoy WiTi world
------
Wait to flash image, and it will boot WiTi-Openwrt Os.
Geeks, let's make more and more interesting things.

