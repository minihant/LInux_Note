# Install and Use Docker on Ubuntu 16.04
## Step 1 — Installing Docker
1. in order to ensure the downloads are valid, add the GPG key for the official Docker repository to your system:
```CCS
$ sudo apt-get install curl -y
$ curl -sSL https://get.docker.com/ | sh
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
	  
2. Add the Docker repository to APT sources:
```
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   or 
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```			
3. Next, update the package database with the Docker packages from the newly added repo:
```
$ sudo apt-get update 
```	
4. Make sure you are about to install from the Docker repo instead of the default Ubuntu 16.04 repo:
```
$ apt-cache policy docker-ce
```  
5. Finally, install Docker:
```
$ sudo apt-get install -y docker-ce
$ sudo apt-get install docker.io
or 
$ sudo apt install docker-ce
```  
6. Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:
```
$ sudo systemctl status docker
   or
$ sudo service docker status
```
 
 
## Step 2 — Executing the Docker Command Without Sudo (Optional)
### If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
```
$ sudo usermod -aG docker ${USER}
```	  
### To apply the new group membership, you can log out of the server and back in, or you can type the following:
```
$ su - ${USER}
```
	  
### you can confirm that your user is now added to the docker group by typing:
```
$ id -nG
```
### If you need to add a user to the docker group that you're not logged in as, declare that username explicitly using:
```
$ sudo usermod -aG docker username
```

## Step 3 — Using the Docker Command

	* The syntax takes this form:
	  $ docker [option] [command] [arguments]
	  
	* To view all available subcommands, type:
	  $ docker
	  
	* To view system-wide information about Docker, use:
	  $ docker info

//=============================================================	  
Step 4 — Working with Docker Images
//=============================================================
	* To check whether you can access and download images from Docker Hub, type:
	  $ docker run hello-world
	  
	* You can search for images available on Docker Hub, For example, to search for the Ubuntu image, type:
	  $ docker search ubuntu
	  
	*  you can download it to your computer using the pull subcommand. Try this with the ubuntu image, like so:
	  $ docker pull ubuntu     	
	  
	* After an image has been downloaded, you may then run a container using the downloaded image with the run subcommand.
	  $ docker run ubuntu  
	
	* To see the images that have been downloaded to your computer, type:
	  $ docker images
	  			 Output:
					EPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
					ubuntu              latest              ea4c82dcd15a        16 hours ago        85.8MB
					hello-world         latest              4ab4c602aa5e        5 weeks ago         1.84kB
   * Remove docker image
		$ docker rmi image-name			

//=================================================================		
Step 5 — Running a Docker Container
//===================================================================
	* As an example, let's run a container using the latest image of Ubuntu. The combination of the -i and -t 
	  switches gives you interactive shell access into the container:
	   $ docker run -it ubuntu
	      Output:
				root@9b0db8a30ad1:/#
				
   * Now you can run any command inside the container.
     # apt-get update
     
   * Then install any application in it. Let's install Node.js:
     # apt-get install -y nodejs
   
   * To exit the container, type exit at the prompt.   

//=====================================================   
Step 6 — Managing Docker Containers
//=====================================================
	* To view the active ones, use:
	  $ docker ps
	  
	* To view all containers — active and inactive — run docker ps with the -a switch:
	  $ docker ps -a
	  
	* To view the latest container you created, pass it the -l switch:
	  $ docker ps -l
	  
	* To start a stopped container, use docker start, followed by the container ID or the container's name. 
	  $ docker start 9b0db8a30ad1 
	  
	* To stop a running container, use docker stop, followed by the container ID or name.
	  $ docker stop hello-world
	  
	* Once you've decided you no longer need a container anymore, remove it with the docker rm command
	  $ docker rm youthful_roentgen
	  
	* You can start a new container and give it a name using the --name switch.
	* You can also use the --rm switch to create a container that removes itself when it's stopped

//=============================================================	
Step 7 — Committing Changes in a Container to a Docker Image
//=============================================================
   * commit the changes to a new Docker image instance using the following command structure:
     $ docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name
        For example:
        $ sudo docker commit -m "added node.js" -a "sammy" d9b100f2f636 sammy/ubuntu-nodejs
        
//=====================================================================        
Step 8 — Pushing Docker Images to a Docker Repository
   * Reference: How To Set Up a Private Docker Registry on Ubuntu 14.04:
      https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-14-04
//========================================================================
	* To push your image, first log into Docker Hub:
		$ Docker login -u docker-registry-username
		
	* If your Docker registry username is different from the local username you used to create the image, 
	  you will have to tag your image with your registry username. For the example given in the last step, you would type:
	   $ docker tag sammy/ubuntu-nodejs docker-registry-username/ubuntu-nodejs
	   
	* Then you can push your own image using:
	   $ docker push docker-registry-username/ubuntu-nodejs
	
	* To push the ubuntu-nodejs image to the sammy repository, the command would be:
		$ docker push sammy/ubuntu-nodejs   

//======================================================		
Step 9 -- Execute docker image linux command
//======================================================
   *  $ docker exec cocker-image-name apt-get update
   
   
//==========================================================  
Quick start a new node red
//============================================================
	sudo docker run -it -p 1880:1880 --name mynodered nodered/node-red-docker 	

//===========================================================	
Quick run an image 
//=============================================================
   sudo docker run -it -p 1880:1880 -p 1883:1883 --name mycontainer yourimage
   sudo docker tag {DOCKER_IMAGE_ID} {YOUR_ACCOUNT_NAME}/{REPOSITORY_NAME}:1.0.0
   
//=============================================================
//Work with docker Hub
//==============================================
1. login to the Docker Hub
   $ sudo docker login
     ==> input your account name & password 
         (minihant , hant8425)
2. tag your image
   $ sudo docker tag image_id  minihant/repository:tag
   for example:
   $ sudo docker tag image_id minihant/hant:1.0.0
   
3. push your tag image
   $ sudo docker push minihant/hant:1.0.0
   
4. pull your docker hub image
   $ sudo docker pull minihant/hant:1.0.0

//======================================================
//-- 儲存 image 成 tar 檔案 -------
// docker save [OPTIONS] IMAGE [IMAGE...]
//======================================================
	 $ sudo docker save busybox > busybox.tar

//=====================================================
//-- 載入 image
// docker load [OPTIONS] -------------
//====================================================
	$ sudo docker load < busybox.tar


//===================================================
//  Docker Volume 
//==================================================
1. 查看目前的 volume
   $ sudo docker volume ls [OPTIONS]

2. 創造一個 volume
   $ sudo docker volume create [OPTIONS] [VOLUME]

3. 刪除一個 volume
   $ sudo docker volume rm [OPTIONS] VOLUME [VOLUME...]

4. 查看 volume 詳細資料
	$ sudo docker volume inspect [OPTIONS] VOLUME [VOLUME...]

5. 移除全部未使用的 volume
	$ sudo docker volume prune [OPTIONS]

6. How to use volume in docker
  $ sudo dockker run -it -v /etc/nginx/sites-available:/etc/nginx/sites-available --name ng nginx /bin/bash
//===================================================
//  Docker network 
//===================================================
1. 查看目前 docker 的網路清單
	$ sudo docker network ls [OPTIONS]
2. 指定 network 範例 ( 指定使用 host 網路 )
	$sudo docker run -it --name busybox --rm --network=host busybox
3. 建立 network
	$sudo docker network create [OPTIONS] NETWORK
4. 移除 network
	$sudo docker network rm NETWORK [NETWORK...]
5. 移除全部未使用的 network
	$sudo docker network prune [OPTIONS]
6. 查看 network 詳細資料
	$sudo docker network inspect [OPTIONS] NETWORK [NETWORK...]
7. 將 container 連接 network
	$sudo docker network connect [OPTIONS] NETWORK CONTAINER

8. User-defined networks
   更多詳細資料可參考 https://docs.docker.com/engine/userguide/networking/#user-defined-networks


//===================================================
//  Docker-Compose 
//  Compose 是定義和執行多 Container 管理的工具
//===================================================
透過 docker-compose.yml ( YML 格式 ) 文件。
docker-compose.yml 的寫法可參考 https://docs.docker.com/compose/compose-file/
也可以直接參考官網範例 https://docs.docker.com/compose/compose-file/#compose-file-structure-and-examples
Compose 的 Command-line 可參考 https://docs.docker.com/glossary/?term=compose
1. 查看目前 Container
	$ sudo docker-compose ps
		加上 -q 的話，只顯示 id
	$ sudo docker-compose ps -q
2. 啟動 Service 的 Container
	$ sudo docker-compose start  [SERVICE...]
3. 停止 Service 的 Container ( 不會刪除 Container )
	$ sudo docker-compose stop [options] [SERVICE...]
4. 重啟 Service 的 Container
	$ sudo docker-compose restart [options] [SERVICE...]
		 ==> Builds, (re)creates, starts, and attaches to containers for a service
	$ sudo docker-compose up [options] [--scale SERVICE=NUM...] [SERVICE...]
 		加個 -d，會在背景啟動，一般建議正式環境下使用。
	$ sudo docker-compose up -d
		up 這個功能很強大，建議可以參考 https://docs.docker.com/compose/reference/up/
5. docker-compose down
	$ sudo docker-compose down [options]
		down 這個功能也建議可以參考 https://docs.docker.com/compose/reference/down/
		舉個例子
			$ sudo docker-compose down -v
				加個 -v 就會順便幫你把 volume 移除（ 移除你在 docker-compose.yml 裡面設定的 volume ）
				在指定的 Service 中執行一個指令
				docker-compose run [options] [-v VOLUME...] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...] [ARGS...]
	舉個例子
		docker-compose run web bash
		在 web Service 中執行 bash 指令
		可參考 https://docs.docker.com/compose/reference/run/
		觀看 Service logs
		docker-compose logs [options] [SERVICE...]

6. 檢查 docker-compose.yml 格式是否正確
	$ sudo docker-compose config
7. 如下指令，和 docker exec 一樣
	$ sudo docker-compose exec
		範例 ( 進入 web 這個 service 的 bash )
				$ sudo docker-compose exec web bash
8. 顯示被使用到的 container 中的 images 清單
	$ sudo docker-compose images
9. 移除 service containers
	$ sudo docker-compose rm
10. Pushes images 到 docker hub
	$ sudo docker-compose push

//==========================================================
install googlg cloud SDK in ubuntu
//==========================================================
	# Create environment variable for correct distribution
	export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"

	# Add the Cloud SDK distribution URI as a package source
	echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

	# Import the Google Cloud Platform public key
	curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

	# Update the package list and install the Cloud SDK
	sudo apt-get update && sudo apt-get install google-cloud-sdk

//=============================================
Use google docker reporisity
//==============================================
1. $ sudo gcloud auth configure-docker

2. 將映像檔推送至註冊資料庫
   您必須先為映像檔加上註冊資料庫名稱的標記，接著再推送映像檔。  	
	 a. 為本機映像檔加上註冊資料庫名稱的標記
	    $ sudo docker tag [SOURCE_IMAGE] [HOSTNAME]/[PROJECT-ID]/[IMAGE]
			 
			sudo docker tag minihant/p1:1.0.0 asia.gcr.io/winpos20190211/mynode

	 b. $ sudo docker push [HOSTNAME]/[PROJECT-ID]/[IMAGE] 
	   
		 sudo docker push asia.gcr.io/winpos20190211/mynode


