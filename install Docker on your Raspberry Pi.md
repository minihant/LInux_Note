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
docker swarm init
7. Once Docker initiates the swarm setup, you will be presented with a command to add additional worker nodes. Below is an example.
docker swarm join --token SWMTKN-1-<token-key> 192.168.93.231:2377
    a. on each worker node paste the text in step 7 
8. To add additional manager nodes, the token and string will be different than the worker string. In order to discover the correct string to add manager nodes, do the following command on an existing working manager node.  
