How to use Google Cloud Build
https://cloud.google.com/cloud-build/docs/quickstart-docker?hl=zh-tw

1. Build a new googlr project. for example "ubuntu-20190622"
2. 新增 quickstart.sh
        ```
        #!/bin/sh
        echo "Hello, world! The time is $(date)."
        ```
3. 新增一個 Dockerfile
  ```
  FROM alpine
  COPY quickstart.sh /
  CMD ["/quickstart.sh"]
  ```
  4. 給 quickstart.sh執行權限
    ```
    chmod +x quickstart.sh
    ```
 5. 開始實作 — 專案建立
  專案名稱  ==> "ubuntu-20190622"
  
6. 開始實作 — 建立 Docker Image
  ```
  gcloud auth login
  ```
  
7. 在SDK console 下, 選擇要用哪個 project，指令如下：
  ```
  gcloud config set project ubuntu-20190622
  ```
  
 8. 創建 docker image 並且 push 到線上的 Container Registry
  ```
  gcloud builds submit --tag gcr.io/ubuntu-20190622/quickstart-image .
  ```
  or 用yaml 創建 docker image:
  創建一個 cloudbuild.yaml
  ```
  steps:
  - name: 'gcr.io/cloud-builders/docker'
    args: [ 'build', '-t', 'gcr.io/$ubuntu-20190622/quickstart-image', '.' ]
  images:
  - 'gcr.io/$ubuntu-20190622/quickstart-image'
  ```
    開始創建 docker image
    ```
    gcloud builds submit --config cloudbuild.yaml .
    ```
    
9. 到 google console 上確認剛剛兩種方式所產生的 image 有沒有正確的被推送到 Container Registry   

10. 開始實作 — 執行 Docker Image
    *. 需要 Container Registry 憑據
      ```
      gcloud auth configure-docker
      ```
    *. 執行剛剛上面步驟所創建好的 docker image
      ```
      docker run gcr.io/[PROJECT_ID]/quickstart-image
      ```
      
    *. 成功的話，結果會出現Hello World

