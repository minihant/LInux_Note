# Cross compiling for ARM with Ubuntu 16.04 LTS

	Build platform: Architecture of the build machine
	Host platform: The architecture you are building for
	Target platform: The architecture that will run the binary output of the compilation

1.	系統必備元件‎
$ sudo apt-get install gcc make gcc-arm-linux-gnueabi 
$ sudo apt-get install binutils-arm-linux-gnueabi
$ sudo apt-get install libc6-armel-cross libc6-dev-armel-cross
$ sudo apt-get install libncurses5-dev
$ sudo apt-get install gcc-arm-linux-gnueabi
$ sudo apt-get install g++-arm-linux-gnueabi

2.	Download ARM Embedded Toolchain from:
https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads
3.	設定 Toolchain 安裝的目錄:
${HOME}/opt/
4.	unpack the archive in the destination folder:
possible locations (${HOME}/local, ${HOME}/opt, /usr/local).
•	$ mkdir -p ${HOME}/opt
•	$ cd ${HOME}/opt
•	$ tar xjf ~/Downloads/gcc-arm-none-eabi-6-2017-q1-update-linux.tar.bz2
•	$ chmod -R -w ${HOME}/opt/gcc-arm-none-eabi-6-2017-q1-update

5.	Test the toolchain path:
$ ${HOME}/opt/gcc-arm-none-eabi-6-2017-q2-update/bin/arm-none-eabi-gcc –version
The complete toolchain documentation is available in the 
.../share/doc/pdf/ folder.
6.	Set Toolchain path
	DO NOT add the toolchain path to the user or system path!
	The GNU ARM Eclipse plug-in has an advanced toolchain path management 
7.	
