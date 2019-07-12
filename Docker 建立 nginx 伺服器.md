## [使用 Docker 建立 nginx 伺服器入門教學](https://blog.techbridge.cc/2018/03/17/docker-build-nginx-tutorial/)

1. 使用官方 nginx image 運行 docker container:
    - 將 nginx image 跑起來成為一個 webserver container，並把 docker container 80 port 對應到本機端的 0.0.0.0:7777
    ```
    $ docker run -d -p 7777:80 --name webserver nginx
    ```

2. 此時在瀏覽器 http://localhost:7777 應該可以看到 nginx 伺服器的首頁

3. 替換 nginx 首頁
    - 在目前資料夾建立一個 index.html 檔案：
    ```
    <!DOCTYPE html>
        <html lang="en">
            <head>
                <title></title>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1">
            </head>
            <body>
                <h1>Hi Nginx Docker</h1>
            </body>
        </html>
    ```

    ```
    $ docker run --name mynginx -p 7777:80 -v index.html:/usr/share/nginx/html:ro -d nginx
    ```

    - 設定檔主檔 /etc/nginx/nginx.conf
    - 預設主機的配置 /etc/nginx/conf.d/default.conf


4. 使用 Dockerfile 建立新的映像檔：
    ```
    FROM nginx
    COPY ./index.html /usr/share/nginx/html
    ```

