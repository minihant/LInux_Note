# How to install Docker on your Raspberry Pi

## the best way moving forward is to use a much simpler method and install directly from get.docker.com. See the example below.
  ```
  curl -sSL https://get.docker.com | sh
  ```
  
## 1. Run apt-get update
  ```
  $ sudo apt-get update
  ```

## 2. Install packages to allow apt to use a repository over HTTPS
  ```
  $ sudo apt-get install apt-transport-https \
                         ca-certificates \
                         software-properties-common
  ```
  
## 3. Add Docker's GPG key

  method 1:
  ```
  $ curl -fsSL https://yum.dockerproject.org/gpg | sudo apt-key add -
  ```
  Verify the correct key id:
  ```
  $ apt-key fingerprint 58118E89F3A912897C070ADBF76221572C52609D
  ```
  Set up the stable repository:
  ```
  $ sudo add-apt-repository \
         "deb https://apt.dockerproject.org/repo/ \
         raspbian-$(lsb_release -cs) \
         main"
  ```
  
  method 2 :
  ### add the line directly to the sources.list file. See below:
  ```
  sudo vim /etc/apt/sources.list
  ```
  ### Append the following:
  ```
  https://apt.dockerproject.org/repo/ raspbian-RELEASE main
  ```
  ### Replace RELEASE with the Raspbian release you're using.
  ### To find your release use:
  ```
  lsb_release -cs
  ```
  
  ## 4. Install Docker
  ```
  $ sudo apt-get update
  $ sudo apt-get -y install docker-engine
  ```
  
  ## 5. Test docker
  ```
  $ sudo docker run hello-world
  ```
