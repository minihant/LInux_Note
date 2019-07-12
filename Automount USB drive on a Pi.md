
# Automount external USB drive on a Raspberry Pi

## Check all the driver:
    ```
    $ sudo blkid
    
    /dev/mmcblk0p1: LABEL_FATBOOT="boot" LABEL="boot" UUID="B6BB-0F0E" TYPE="vfat" PARTUUID="d17d4ea5-01"
    /dev/mmcblk0p2: LABEL="rootfs" UUID="638417fb-7220-47b1-883c-e6fee02f51ac" TYPE="ext4" PARTUUID="d17d4ea5-02"
    /dev/mmcblk0: PTUUID="d17d4ea5" PTTYPE="dos"
    /dev/sda1: LABEL="Transcend" UUID="469E6F909E6F76F9" TYPE="ntfs" PARTUUID="a4243e6a-01"
    /dev/sda2: LABEL="git"       UUID="E2CC3ABCCC3A8B35" TYPE="ntfs" PARTUUID="a4243e6a-02"

    ```

## Create a location for the external drive mount point:    
    ```
    $ sudo mkdir /mnt/hd-1
    $ sudo mkdir /mnt/hd-git
    ```

## Give to it right permissions:
    ```
    $ sudo chmod 770 /mnt/hd-1
    $ sudo chmod 770 /mnt/hd-git
    ```

## Mount the drive and check if it is working:
    ```
    $ sudo mount /dev/sda1 /mnt/hd-1
    $ sudo mount /dev/sda2 /mnt/hd-git
    $ ls /mnt
    ```

## Take a backup of current fstab and then edit it to do external drive auto-mounting:
    
    ### backup fstab file
    ```
    $ sudo cp /etc/fstab /etc/fstab.backup
    ```
    
    ### Edit fstab file
    ```
    $ sudo vim /etc/fstab
    ```

## Add the correct mount information in the fstab file:
    ```
    UUID=1E2CC3ABCCC3A8B35 /mnt/exthd ext4 defaults 0 0    

    ```
## final 
    ```
    $ sudo reboot
    ```
