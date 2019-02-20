# Running node-red under Docker
## Container versions
    1. latest - uses official Node.JS v4 base image.
    2. slim - uses Alpine Linux base image.
    3. rpi - uses RPi-compatible base image.
        - Using Alpine Linux reduces the built image size (~100MB vs ~700MB) but removes standard dependencies
            that are required for native module compilation. If you want to add modules with native dependencies, use the standard image or extend the slim image with the missing packages.
        - Additional images using a newer Node.js v8 base image are now available with the following tags.
            1. latest-v8
            2. slim-v8
            3. rpi-v8

## Quick start
```
docker run -it -p 1880:1880 --name mynodered nodered/node-red-docker
```
    - Note: on a Raspberry Pi you must use a rpi-tagged image: nodered/node-red-docker:rpi.
    - This command will download the nodered/node-red-docker container from DockerHub and run an instance      of it with the name of mynodered and with port 1880 exposed. In the terminal window you will see         Node-RED start. Once started you can then browse to http://{host-ip}:1880 to access the editor.
    Hit Ctrl-p Ctrl-q to detach from the container. This leaves it running in the background.

## To reattach to the container
```
docker attach mynodered
```

## To stop the container:
```
docker stop mynodered
```

## To start the container:
```
docker start mynodered
```

## Note : your flows will be stored in the file called flows.json within the container. This can be customised by setting the FLOWS environment parameter: 
```
docker run -it -p 1880:1880 -e FLOWS=my_flows.json nodered/node-red-docker
```

## Node.js runtime arguments can be passed to the container using an environment parameter (NODE_OPTIONS). For example, to fix the heap size used by the Node.js garbage collector you would use the following command:
```
docker run -it -p 1880:1880 -e NODE_OPTIONS="--max_old_space_size=128" nodered/node-red-docker
```

## Customising
    - The container uses the directory /data as the user configuration directory. To add additional nodes you can open shell into the container and run the appropriate npm install commands:
    
## Open a shell in the container
```
docker exec -it mynodered /bin/bash
```

## Once inside the container, npm install the nodes in /data
```
    cd /data
    npm install node-red-node-smooth
    exit

    # Restart the container to load the new nodes
    docker stop mynodered
    docker start mynodered
```
## Storing data outside of the container
```
docker run -it -p 1880:1880 -v ~/node-red-data:/data --name mynodered nodered/node-red-docker
```
- This command mounts the host’s ~/node-red-data directory as the user configuration directory inside the container.

- Adding extra nodes to the container can then be accomplished by running npm install on the host machine:
```
cd ~/node-red-data
npm install node-red-node-smooth
docker stop mynodered
docker start mynodered
```
- Note : Modules with a native dependencies will be compiled on the host machine's architecture. These modules will not work inside the Node-RED container unless the architecture matches the container's base image. For native modules, it is recommended to install using a local shell or update the package.json and re-build.

## Building the container from source
1. To build your own version:
```
git clone https://github.com/node-red/node-red-docker.git
cd node-red-docker

# Build it with the desired tag
docker build -f <version>/Dockerfile -t mynodered:<tag> .
```

## Building a custom image
- Creating a new Docker image, using the public Node-RED images as the base image, allows you to install extra nodes during the build process

    1. Create a file called Dockerfile with the content:
    2. FROM nodered/node-red-docker
    3. RUN npm install node-red-node-wordpos
    4. Run the following command to build the image:
    5. docker build -t mynodered:<tag> .
    - That will create a Node-RED image that includes the wordpos nodes.

## Updating container
```
docker pull nodered/node-red-docker
docker stop mynodered
docker start mynodered
```

## Linking Containers
- You can link containers “internally” within the Docker runtime by using the --link option.
- For example, if you have a container that provides an MQTT broker container called mybroker, you can run the Node-RED container with the link parameter to join the two:
```
docker run -it -p 1880:1880 --name mynodered --link mybroker:broker nodered/node-red-docker
```
- This will make broker a known hostname within the Node-RED container that can be used to access the service within a flow, without having to expose it outside of the Docker host.

