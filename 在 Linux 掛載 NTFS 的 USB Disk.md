# 在 Linux 掛載 NTFS 的 USB Disk
  ## : https://snippetinfo.net/media/238
  
## 1. 先到以下網址下載最新的 source code
    http://www.tuxera.com/community/ntfs-3g-download/  
    
## 2. 傳到 Linux 上之後，就解開，並執行以下動作：
  ```
  ./configure
  make
  make install # or 'sudo make install' if you aren't root
  ```
## 3. 等安裝好後，可以透過 fdisk -l 找出該 USB Disk 中的 代號
  ```
  fdisk -l
  ```
  mount -v -t ntfs-3g /dev/sda2 /mnt/usb
