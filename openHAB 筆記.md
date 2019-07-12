### 1. Install openhab2
```
wget -qO - 'https://bintray.com/user/downloadSubjectPublicKey?username=openhab' | sudo apt-key add – 
echo 'deb http://dl.bintray.com/openhab/apt-repo2 stable main' | sudo tee /etc/apt/sources.list.d/openhab2.list
sudo apt-get update
sudo apt-get install openhab2
sudo systemctl start openhab2.service
sudo systemctl status openhab2.service  
sudo systemctl daemon-reload
sudo systemctl enable openhab2.service
```

### 2. Configuration openhab
```
   $ Sudo nano /etc/default/openhab
- Change USER_AND_GROUP=pi:pi
    Sudo nano /usr/lib/systemd/system/openhab.service
- Change User = pi
- Change Group=pi
   $ Sudo systemctl daemon-reload
   $ Sudo service openhab restart
   $ Sudo nano /etc/openhab/configurations/items/home.items
- Add
- Switch RaspiLED{ gpio=”pin:4”}
   $ Sudo nano /etc/openhab/configurations/sitemaps/home.sitemap
- Add
    sitemap home label=”HOME”
    {
        Fame lable=”Raspberry Pi GPIO LED”
        {
            Switch item=RaspiLED
        }
    } 
    Sudo service openhab restart
```

### 3. Open Webbrowser
    - 192.168.38.109:8080/openhab.app?sitemap=home

### 4. Uninstall openhab
```
sudo apt-get purge openhab2*
sudo rm /etc/apt/sources.list.d/openhab2.list
```

## reference:
[openHAB 2 on Linux](http://docs.openhab.org/installation/linux.html)
