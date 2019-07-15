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
  ### Install Docker compose
  ```
  $ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  
  $ sudo chmod +x /usr/local/bin/docker-compose
  ```
  or
  ```
  $ sudo apt-get install python-pip
  $ sudo pip install docker-compose
  ```
  
  
  # 這是一個創建 ubuntu 並安裝 nginx 的 image
  ```
  FROM ubuntu:16.04 # 從 Docker hub 下載基礎的 image，可能是作業系統環境或是程式語言環境，這邊是 ubuntu 16.04
  MAINTAINER demo@gmail.com # 維護者
  RUN apt-get update # 執行 CMD 指令跑的指令，更新 apt 套件包資訊
  RUN apt-get install –y nginx # 執行 CMD 指令跑的指令，安裝 nginx
  CMD ["echo", "Nginx Image created"]
  ```
  ## 透過 Docker 建立 Python Pageview App
  ### 透過一個簡單 Python Flask + Redis 網頁人數統計的專案讓讀者可以更深刻理解 Docker Compose 的威力。
  ### 1. 創建專案資料夾
    ```
    $ mkdir counter
    $ cd counter
    ```
  ### 2. 在資料夾下建立 app.py 當做 web app 進入點
  #### 當使用者瀏覽首頁時，redis 會記錄次數，若有 exception 則有 retry 機制
    ```
    import time
    import redis
    from flask import Flask

    app = Flask(__name__)
    cache = redis.Redis(host='redis', port=6379)

    def get_hit_count():
        retries = 5
        while True:
            try:
                return cache.incr('hits')
            except redis.exceptions.ConnectionError as exc:
                if retries == 0:
                    raise exc
                retries -= 1
                time.sleep(0.5)

    @app.route('/')
    def get_index():
        count = get_hit_count()
        return 'Yo! 你是第 {} 次瀏覽\n'.format(count)

    if __name__ == "__main__":
        app.run(host="0.0.0.0", debug=True)
        
    ```
  ### 3. 建立套件 requirements.txt 安裝資訊讓 Dockerfile 可以下指令安裝套件
    ```
    1. flask
    1. redis
    ```
  ### 4. 建立 Web App 的 Dockerfile
    ```
    FROM python:3.4-alpine # 從 python3.4 基礎上加工
    ADD . /code # 將本地端程式碼複製到 container 裡面 ./code 資料夾
    WORKDIR /code # container 裡面的工作目錄
    RUN pip install -r requirements.txt
    CMD ["python", "app.py"]
    ```
  ### 5. 用 Docker Compose file 描述 services 運作狀況，我們的專案共有 web 和 redis 兩個 service
    ```
    version: '3'
    services:
    web:
        build: .
        ports:
        - "5000:5000"
        volumes:
        - .:/code # 把當前資料夾 mount 掛載進去 container，這樣你可以直接在本地端專案資料夾改動檔案，container 裡面的檔案也會更動也不用重新         build image！
    redis:
        image: "redis:alpine" # 從 Docker Hub registry 來的 image
    ```
  ### 6. 用 Docker Compose 執行你的 Web app
    ```
    $ docker-compose up -d
     -d detached 是在背景執行
     
    $ docker ps -a 觀看目前所有 docker container 狀況
    $ docker-compose ps 觀看 docker-compose process 狀況
    $ docker-compose down 終止並移除 container 
    ```
  ### 7. 到 http://127.0.0.1:5000/ 觀看成果
  
  
  
  
