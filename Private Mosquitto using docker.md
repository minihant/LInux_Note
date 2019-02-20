# Private Mosquitto using docker 

## Install docker in AWS or GCP
```
sudo yum/apt install docker 
sudo service docker start

```

## Install mosquitto
```
sudo docker pull toke/mosquitto
sudo docker run --name mqtt toke/mosquitto
```
- We started the docker container without mapping any port or volumes
- we can still do something interesting with it to test
```
sudo docker exec -i -t mqtt /bin/bash
```
remov it
```
sudo docker stop mqtt 
sudo docker rm mqtt
```

- Let’s create some empty folders that we will use later as docker volumes:
```
sudo mkdir -p /srv/toke-mosquitto/{config,data,log}
```

## Obtaining a certiﬁcate
-  set up a secure channel. We need a certiﬁcate.
-  use this https://letsencrypt.org/
```
sudo yum install epel-release nginx
```

- We conﬁgure our virtualhost in /etc/nginx/conf.d/virtual.conf
```
sudo service nginx restart 
wget https://dl.eff.org/certbot-auto 
chmod a+x certbot-auto ./certbot-auto --nginx -d <my-domain> certonly [--debug]
```
- Our new certiﬁcate should now be in /etc/letsencrypt folder.

- We will need to copy the certiﬁcates in a place accessible by docker:
```
sudo cp -a /etc/letsencrypt /srv/toke-mosquitto/config/
```

- Following the instructions here
[instructions here](https://mosquitto.org/2015/12/using-lets-encrypt-certiﬁcates-withmosquitto/)
- we also create the chain-ca.pem ﬁle needed by mosquitto:
```
cd /srv/toke-mosquitto/config 
wget https://letsencrypt.org/certs/isrgrootx1.pem 
sudo bash -c 'cat /letsencrypt/live/<mydomain>/chain.pem is rgrootx1.pem > chain-ca.pem'
```

## Conﬁguring Mosquitto
-  we can put our own mosquitto.conf and access control ﬁles.
```
listener 8883 
allow_anonymous false 
password_file /mqtt/config/pwfile 
cafile /mqtt/config/chain-ca.pem 
certfile /mqtt/config/letsencrypt/live/<my-domain>/cert.pem 
keyfile /mqtt/config/letsencrypt/live/<my-domain>/privkey.pem
```

- To create the missing pwfile we can use the mosquitto_pass utility
```
sudo docker exec -i -t mqtt /bin/bash
```

-  on AWS we also need to open port 8883 in the security group to make it reachable

## Ready to start
```
sudo docker run -ti -p 8883:8883 -v /srv/toke-mosquitto/con fig/:/mqtt/config:ro -d --name mqtt toke/mosquitto
```

- test that it works from our PC by issuing the following command:
```
mosquitto_pub -h <my-domain> -t test/topic -m test -p 8883 --capath /etc/ssl/certs/
```

- To troubleshoot issue with SSL we can also use
```
wget https://letsencrypt.org/certs/isrgrootx1.pem openssl s_client -connect <my-domain>:8883 -CAfile isrgroot x1.pem
```
