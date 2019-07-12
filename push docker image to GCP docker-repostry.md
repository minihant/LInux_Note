
```
$ sudo gcloud auth login
$ sudo docker tag minihant/mqtt:1.0.2 gcr.io/winpos20190211/mymqtt$  
$ sudo gcloud docker -- push gcr.io/winpos20190211/mqtt:1.0.2
$ sudo gcloud docker -- pull gcr.io/winpos20190211/mqtt:1.0.2
```

Configuring SSL
1.Create a private key
  $ sudo openssl genrsa -out node-key.pem 2048
  
2. Create a certificate Request
  $ sudo openssl req -new -sha256 -key node-key.pem -out node-csr.pem
  
3. Sign the Certificate with the Private key to create a self signed Certificate
  $ sudo openssl x509 -req -in node-csr.pem -signkey node-key.pem -out node-cert.pem

4. un-comment the line in setting.js
    var fs=require("fs")
    .....
    https{
     key:  fs.readFileSync('node-key.pem'),
     cert: fs.readFileSync('node-cert.pem')
    }
    
    requireHttps: true,
    
 5. put the .pem files into th directory of "/usr/src/node-red"   

A. 建立反向代理 (Reverse Proxy)
  sudo docker run \
    --name reverse-proxy \
    -v $HOME/nginx/certs:/etc/nginx/certs:ro \
    -v $HOME/nginx/vhost.d:/etc/nginx/vhost.d \
    -v $HOME/nginx/html:/usr/share/nginx/html \
    -v $HOME/nginx/conf.d:/etc/nginx/conf.d \
    -v /var/run/docker.sock:/tmp/docker.sock:ro \
    --label com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy=true \
    -p 80:80 \
    -p 443:443 \
    -d \
    --restart unless-stopped \
    jwilder/nginx-proxy

B. 訂閱 Let’s Encrypt 憑證
sudo docker run \
    --name letsencrypt \
    --volumes-from reverse-proxy \
    -v $HOME/nginx/certs:/etc/nginx/certs:rw \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -d \
    --restart unless-stopped \
    --network bridge \
    jrcs/letsencrypt-nginx-proxy-companion


建立 nginx 網頁伺服器並指定給 minicow.ddnsfree.com
sudo docker run \
    --name site1 \
    -e 'VIRTUAL_HOST=minicow.ddnsfree.com' \
    -v $HOME/nginx/conf.d:/etc/nginx/conf.d \
    -p 80:80 \
    -d \
    --restart always\
    --network bridge \
    nginx:latest


alpine

server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
