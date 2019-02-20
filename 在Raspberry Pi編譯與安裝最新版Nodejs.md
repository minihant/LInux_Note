# 在Raspberry Pi編譯與安裝最新版Node.js
## Node.js官網已經有提供事先編譯好的ARM版本，可直接給Raspberry Pi使用。例如，底下網址包含已編譯好的Node.js全部版本：
    ```
    https://nodejs.org/dist/
    例如，其中的”latest-v4.x”路徑代表「最新4.x版」，適用於Raspberry Pi的是檔名以armv71結尾的檔案。
    ```
## 以下載、安裝4.7.2版為例，請在終端機視窗輸入下列命令（#號與後面的文字代表註解，不用輸入）：
    ```
    wget https://nodejs.org/dist/latest/node-v4.7.2-linux-armv7l.tar.gz    # 下載檔案
    tar -xvf node-v4.7.2-linux-armv7l.tar.gz        # 解壓縮
    cd node-v4.7.2-linux-armv7l    # 切換到目錄
    sudo cp -R * /usr/local/     # 複製所有檔案到 /usr/local/ 路徑

    重新啟動樹莓派
    ```
## 用一個簡單的HTTP伺服器程式測試看看：
    ```
    const http = require('http');
    const port = 8080;

    http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Hello World</h1>');
    }).listen(port, () => {
        console.log('本機網站伺服器在 ${port} 埠啟動了！');
    });
    ```    
### use browser to open server http://127.0.0.0:8080    

