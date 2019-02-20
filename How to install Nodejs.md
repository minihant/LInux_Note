# How to Turn Your Raspberry Pi into a Development Server
[ref](https://www.toptal.com/raspberry-pi/how-to-turn-your-raspberry-pi-into-a-development-server)
	## Install nodejs from package manager ( for PC )
	- nodejs 6 :
	```
	curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
	sudo apt-get install -y nodejs
	```

	- nodejs 8  ( for ARM base V8:64bits)
	```
	curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
	sudo apt-get install -y nodejs
	```
	## Remove nodejs
	```
	node -v
		v0.10.29
	sudo su -
	apt-get remove nodered -y
	apt-get remove nodejs nodejs-legacy -y
	apt-get remove npm  -y # if installed npm
	curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
	apt-get install nodejs -y
	node -v
		v6.11.0
	npm -v
		3.10.10
	```

	## Install nodejs from package manager ( for ARM )
	```
	mkdir node
	cd node
	wget https://nodejs.org/dist/v6.11.1/node-v6.11.1-linux-arm64.tar.xz
	tar -xvf node-v6.11.1-linux-arm64.tar.xz
	cd node-v6.11.1-linux-arm64
	sudo cp -R * /usr/local/
	sudo reboot
	node -v
		v6.11.1
	npm -v
		3.8.6
	```

	## How to Turn Your Raspberry Pi Into a Development Server
[ref:](https://www.toptal.com/raspberry-pi/how-to-turn-your-raspberry-pi-into-a-development-server)

	## Node Bluetooth serial port
[ref:](https://github.com/eelcocramer/node-bluetooth-serial-port)

	## NodeJS Serial port
[ref](https://github.com/ITPNYU/physcomp/tree/master/labs2014/Node%20Serial%20Lab)
[ref:](https://itp.nyu.edu/physcomp/labs/labs-serial-communication/lab-serial-communication-with-node-js/#Using_theData_in_HTML)

	## Nodejs install JAVA
	```
	npm install java
	npm init  
		- creates a package.json in root for you
	npm list
		- lists all installed packages
	npm prune
		- removes packages not depended on by your project according to your package.json
	npm outdated 
		- tells you which installed packages are outdated with respect to what is current in the npm registry but allowable by the version definition in your package.json
	```

	## npm check update
	```
	npm i -g npm-check-updates
	npm-check-updates -u
	npm install
	```

	## Nodejs call java API
[ref:](https://github.com/joeferner/node-java)

