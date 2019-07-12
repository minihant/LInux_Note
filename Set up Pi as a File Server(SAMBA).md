# Samba: Set up a Raspberry Pi as a File Server for your local network
  ref: https://www.raspberrypi.org/magpi/samba-file-server/
  
## File Server: Set up Samba
  ```
  $ sudo apt-get update
  $ sudo apt-get upgrade
  $ sudo apt-get install samba samba-common-bin
  ```
  
## Create your shared directory:
  ```
  $ sudo mkdir -m 1777 /share
  ```
## Edit Samba’s config files to make the file share visible to the Windows PCs on the network.
  ```
  $ sudo vim /etc/samba/smb.conf
  ```
  
  add the following entry:
  ```
  [share]
    Comment = Pi shared folder
    Path = /share
    Browseable = yes
    Writeable = Yes
    only guest = no
    create mask = 0777
    directory mask = 0777
    Public = yes
    Guest ok = yes
  ```
   ### If you don’t want to allow guest users, omit the guest ok = yes line.
  
## Create a user and start Samba
  ```
  $ sudo smbpasswd -a pi
  ```
  Then set a password as prompted. Finally, let’s restart Samba:
  ```
  $ sudo /etc/init.d/samba restart
  or
  $ sudo systemctl restart smbd.service
  ```

## Find your Pi on the network 
  ### You’ll now be able to find your Raspberry Pi file server (named RASPBERRYPI by default) 

## 檢查設定檔語法
只要有修改 smb.conf，最好都檢查一下語法是否正確：
  ```
  $ sudo testparm
  ```
## 新增帳戶
  新增 pi 帳戶，並設定密碼：
  ```
  $ sudo pdbedit -a -u pi
    new password:
    retype new password:  
  ```
  
## 查詢帳戶
  查詢目前存在 Samba 的帳戶 ：
  ```
  $ sudo pdbedit -L
  ```
  
## 修改帳戶密碼  
修改已存在 Samba 裡的帳戶 pi 的密碼：
  ```
  $ sudo smbpasswd pi
  ```
 
## 刪除帳戶
刪除已存在 Samba 裡的帳戶 pi：
  ```
  $ sudo pdbedit -x -u pi 
  ```
