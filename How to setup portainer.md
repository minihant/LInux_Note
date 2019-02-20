# Docker 搭建 Portainer 視覺化介面
[Docker(七)—-搭建Portainer視覺化介面](https://codertw.com/程式語言/631088/)

   ## Install portainer docker image
   ```
   $ sudo docker pull portainer/portainer
   ```

   1. 單機版執行
   ```
   sudo docker volume create portainer_data
   sudo docker run -d -p 9000:9000 \
   --restart=always \
        -v /var/run/docker.sock:/var/run/docker.sock \
        --name portainer-test \
        portainer/portainer

   or
   sudo docker run -d -p 9000:9000 \
   --restart=always \
   -v /var/run/docker.sock:/var/run/docker.sock
   -v portainer_data:/data \
   --name=portainer-1 \
   portainer/portainer
   ```

   2. 叢集執行
   ```
   sudo docker run -d -p 9000:9000 --restart=always --name portainer-1 portainer/portainer
   選擇Remote :
   ```
