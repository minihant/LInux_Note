# How to install Docker on your Raspberry Pi


## a. Install the following prerequisites:
  ```
  $ sudo apt-get install apt-transport-https ca-certificates software-properties-common -y
  ```
## b. Download and install Docker.
  ```
  $ curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
  ```
  
## c. Give the ‘pi’ user the ability to run Docker
  ```
  $ sudo usermod -aG docker pi
  ```
## d. Import Docker CPG key.
  ```
  $ sudo curl https://download.docker.com/linux/raspbian/gpg
  ```
## e. Setup the Docker Repo.
  ```
  $ vim /etc/apt/sources.list
         Add the following line and save:   
    deb https://download.docker.com/linux/raspbian/ stretch stable
  ```  
  
## f. Patch and update your Pi.
  ```
  $ sudo apt-get update
  $ sudo apt-get upgrade
  ```
## g. Start the Docker service.
  ```
  systemctl start docker.service
  ```
  
## h. To verify that Docker is installed and running.
  ```
  docker info
  ```
## i. You should now some information in regards to versioning, runtime,etc.
5. Now that Docker has been installed on all of your Pi’s, we can now setup Docker Swarm.
6. On one of your Pi devices that will be a master node, type the following:
  ```
  $ sudo docker swarm init
  ```
7. Once Docker initiates the swarm setup, you will be presented with a command to add additional worker nodes. 
  Below is an example.
  ```
  $ sudo docker swarm join --token SWMTKN-1-<token-key> 192.168.93.231:2377
  ```
    a. on each worker node paste the text in step 7 
8. To add additional manager nodes, the token and string will be different than the worker string. 
  In order to discover the correct string to add manager nodes, 
  do the following command on an existing working manager node. 
  ```
  $ sudo docker swarm join-token manager
    a. copy and paste the output to each of your manager nodes
  ```
9. If you want to add additional worker nodes and don’t have the correct syntax, just type the following on any of the working manager nodes to retrieve it.
docker swarm join-token worker
10. To have a graphical representation of your current cluster, we will install the VIZ application. For more information, go to https://github.com/dockersamples/docker-swarm-visualizer. To install, type the following:
docker swarm join-token worker \
--name=viz \
--publish=9090:8080/tcp \
--constraint=node.role==manager \
--mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
alexellis2/visualizer-arm:latest
11. Using a browser, connect to one of your master services on port 9090. You should now see the Visualizer showing your worker and manager nodes.
12. Now we will install the monitor app that will be deployed on both the manager and worker nodes. Type the following on the one of the manager nodes.
docker service create --name monitor --mode global \
--restart-condition any --mount type=bind,src=/sys,dst=/sys \
--mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
stefanscherer/monitor:1.2.0
13. Once the monitor app is install, we will now install the ‘whoami’ app. The ‘whoami’ app is a small application that will trigger the LED’s on/off by scaling the application up and down. For each running instance, you will get one LED turned on. As we scale the application up to 5, you will have 5 LEDs turn on. As you scale up and down the number of LEDs that turn on will depend on how many containers you have running in your cluster. To install the application, type the following.
docker service create --name whoami stefanscherer/whoami:1.1.0
14. Once deployed, you should have 1 LED turned on.
15. Now lets scale the application to 5. Type the following.
docker service scale whoami=5
16. You should now have 5 LEDs on. Please that this will take some time as the Pi devices are not very fast and require some time to properly deploy and bring up.
And there you go! We hope you have fun and enjoy some Pi(e) today! If you have feedback or suggestions on how to improve, please reach out to me on Twitter: @paulofrazao or on Github: paulofrazao.
