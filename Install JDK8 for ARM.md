# Install JDK8 for ARM
1.	Download JDK 8
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
2.	進行底下的步驟：
	$ sudo mkdir -v -v /opt/java
	$ sudo tar zxvf ~/jdk-8u33-linux-arm-vfp-hflt.tar.gz -C /opt/java
	解壓縮並把所有東西放進/opt/java裡的子目錄jdk1.8.0_33
	$ sudo update-alternatives --install "/usr/bin/java" "java" "/opt/java/jdk1.8.0_33/bin/java" 1
	$ sudo update-alternatives --set java /opt/java/jdk1.8.0_33/bin/java

3.	How to download and install prebuilt OpenJDK packages
	JDK 8
$ sudo apt-get install openjdk-8-jre
$ sudo apt-get install openjdk-8-jdk
$sudo update-alternatives --config java
$sudo update-alternatives --config javac

 
CUPS install thermal printer 參考網站
https://learn.adafruit.com/networked-thermal-printer-using-cups-and-raspberry-pi/network-printing

