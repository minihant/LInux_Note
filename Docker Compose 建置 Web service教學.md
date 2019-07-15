# Docker Compose 建置 Web service 起步走入門教學
## Docker Compose 簡介:
  ```
  我們在開發一個典型的 Web project 時通常不是只有一個 service，
  有可能需要 app server、database、cache，甚至是 reverse proxy 等 service 
  才能構成一個可以上線運行的專案，這些 service 往往會需要多個 container 來運行.
  這時候就是 Docker Compose 發揮功能的時候啦！
  ```

## 一個基本的 docker-compose.yml 檔案
  ```
  version: '3' # 目前使用的版本，可以參考官網：
  services: # services 關鍵字後面列出 web, redis 兩項專案中的服務
  web:
    build: . # Build 在同一資料夾的 Dockerfile（描述 Image 要組成的 yaml 檔案）成 container
    ports:
      - "5000:5000" # 外部露出開放的 port 對應到 docker container 的 port
    volumes:
      - .:/code # 要從本地資料夾 mount 掛載進去的資料
    links:
      - redis # 連結到 redis，讓兩個 container 可以互通網路
  redis:
    image: redis # 從 redis image build 出 container
  ```
  
## Dockerfile 和 Docker Compose 的差異是？
  Docker Compose 主要是用來描述 Service 之間的相依性和調度方式後，
  ### Dockerfile 是用來描述映像檔（image）的文件:
  ```
  所謂的 Image，就是生產 Container 的模版，你可以從 Docker Hub 
  官方下載或是根據官方的 Image 自己加工後打包成 Image 或是完全自己使用 Dockerfile 
  描述 Image 內容來製作 Image。
  ```
  ### 而 Container 則是透過 Image 產生隔離的執行環境，
  ```
  稱之為 Container，也就是我們一般用來提供 microservice 的最小單位。
  ```
  
  ```
  # 這是一個創建 ubuntu 並安裝 nginx 的 image
  FROM ubuntu:16.04 # 從 Docker hub 下載基礎的 image，可能是作業系統環境或是程式語言環境，這邊是 ubuntu 16.04
  MAINTAINER demo@gmail.com # 維護者

  RUN apt-get update # 執行 CMD 指令跑的指令，更新 apt 套件包資訊
  RUN apt-get install –y nginx # 執行 CMD 指令跑的指令，安裝 nginx
  CMD ["echo", "Nginx Image created"]
  ```
  
