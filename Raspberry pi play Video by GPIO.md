# Raspberry Pi play video by using GPIO

## Play Video With Python and GPIO
[Ref:](https://www.hackster.io/ThothLoki/play-video-with-python-and-gpio-a30c7a)
   
   1. Create a file "videoplayer.py"
   2. edit the file as:
      ```
      import Rpi.GPIO as GPIO
      import sys
      import os from subprocess import Popen
      GPIO.setmode(GPIO.BCM)
      GPIO.setup(17, GPIO.IN, pull_up_down=GPIO.PUD_UP)
      GPIO.setup(18, GPIO.IN, pull_up_down=GPIO.PUD_UP)
      GPIO.setup(24, GPIO.IN, pull_up_down=GPIO.PUD_UP) 
      movie1 = ("/home/pi/Videos/movie1.mp4")
      movie2 = ("/home/pi/Videos/movie2.mp4")
      last_state1 = True
      last_state2 = True
      input_state1 = True
      input_state2 = True
      quit_video = True
      while True:
      #Read states of inputs
      input_state1 = GPIO.input(17)
      input_state2 = GPIO.input(18)
      quite_video = GPIO.input(24)

      #If GPIO(17) is shorted to ground
      if input_state1 != last_state1:
         if (player and not input_state1):
            os.system('killall omxplayer.bin')
            omxc = Popen(['omxplayer', '-b', movie1])
            player = True
         elif not input_state1:
         omxc = Popen(['omxplayer', '-b', movie1])
         player = True

      #If GPIO(18) is shorted to ground
      elif input_state2 != last_state2:
         if (player and not input_state2):
            os.system('killall omxplayer.bin')
            omxc = Popen(['omxplayer', '-b', movie2])
            player = True
         elif not input_state2:
            omxc = Popen(['omxplayer', '-b', movie2])
            player = True

      #If omxplayer is running and GPIO(17) and GPIO(18) are NOT shorted to ground
      elif (player and input_state1 and input_state2):
         os.system('killall omxplayer.bin')
         player = False

      #GPIO(24) to close omxplayer manually - used during debug
      if quit_video == False:
         os.system('killall omxplayer.bin')
         player = False

      #Set last_input states
      last_state1 = input_state1
      last_state2 = input_state2 
      ```
   3. Execute by input : python3 videoplayer.py

   
## omxplayer-player.py    	
[jehutting/omxplayer-player](https://github.com/jehutting/omxplayer-player/blob/master/omxplayer-player.py)

[Push button play video or MP3](http://www.pibeginners.com/playing-media-via-cli-gpio/)
   

## Nodejs play video
[Node-Omxplayer](https://www.npmjs.com/package/node-omxplayer)

[Module omx-manager](https://npm.taobao.org/package/omx-manager#othermethods)
   
   1. npm install node-omxplayer
   2. sudo apt-get install omxplayer
   3. edit app.js
      ```
      var Omx = require('node-omxplayer');
      var player = Omx('/home/pi/Videos/movie1.mp4', "both", "true");
      ```
   4. API:
      ```
      player.volUp()
      player.volDown()
      player.fastFwd()
      player.rewind()
      player.fwd30()
      player.back30()
      player.quit() 
      player.running
         Boolean giving the playback status, 
         true if the player is still active, 
         false if it has ended or the player has quit.
      ```   

## forever service 
[ref:](https://github.com/zapty/forever-service)
   ```
   npm install -g forever
   npm install -g forever-service  
   ```
   1. Usage :
   ```
      $ forever-service --help 
   ```
   2. Install new service :
   ```
      $ forever-service install --help 
      
      for example:
      $ sudo forever-service install test
        this will install local folder app.js
   ```   
   3. Delete service :
   ```
      $ forever-service delete --help
   ```
   4. List service
   ```
      $ sudo forever list
   ```
   5. Sart/Stop service
   ```
      $ sudo service test start
      $ sudo service test sttop
      $ sudo service test restart
      $ sudo service test status
   ```
      

## Module omx-manager
   ```
   npm install omx-manager
   ```
   1. Usae:
   ```
      var OmxManager = require('omx-manager');
      var manager = new OmxManager(); // OmxManager
      var camera = manager.create('video.avi'); // OmxInstance
      camera.play(); // Will start the process to play videos  
   ```
   2. Multi File :
   ```
   manager.create(['video.avi', 'anothervideo.mp4', 'video.mkv']); 
   ```   
   3. set Omx command
   ```
      manager.setOmxCommand('/path/to/my/command');
   ```
   
   4. Events :
   ```
      camera.on('play', function(video) {});  
      camera.on('pause', function() {});
      camera.on('stop', function() {});
      camera.on('end', function() {}); 
   ```
    
   5. �z�i�H�o�X�H�U�R�O?����x��?�e�M��?�¦�G 
      /usr/bin/tvservice -p 
      ���\�A��omxplayer��?��u?�b?�v��?��?��¦�C 
      

## Display Photo by FEH
[ref:](https://pimylifeup.com/raspberry-pi-photo-frame/)
   
   ```
   sudo apt-get install feh
   ```
   1. input command :
   ```
      DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh -q -p -Z -F -R  60 -Y -D 1.0 /home/pi/Pictures/
   ```
   2. run at bootup
   ```
   sudo nano /etc/rc.local
      Add the following before the exit 0 line in this folder.
         sleep 10
         bash /home/pi/start-picture-frame.sh &
   ```
   3. stop the feh
   ```
      sudo pkill feh
   ```
   4. HDMI on
   ```
      xset -display :0.0 dpms force on
   ```
      
## Display Photo by FBI
[ref:](http://ofbrooklyn.com/2014/01/2/building-photo-frame-raspberry-pi-motion-detector/)

   ```
   sudo apt-get install fbi
   ```
   1. input command :
   ```
       sudo fbi -T 2 -noverbose -m 640x480 -a -u -t 2 /home/pi/Pictures/*  
   ```


## reinsttall nodejs 
   ```
   curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
   sudo apt-get install -y nodejs
   npm install -g npm@latest
   ```

## raspberry pi node install serialport
   ```
   sudo npm install -g node-gyp
   sudo npm install -g node-pre-gyp  
   sudo npm install serialport --unsafe-perm    
   ```   