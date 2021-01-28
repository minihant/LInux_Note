# Build Code-server with container im WSL2

> https://github.com/cdr/code-server/issues/1868

## 事實證明，添加自動UPNP防火牆端口映射非常容易，這樣它就可以通過NAT暴露出來，而無需手動配置路由器防火牆，而僅通過主機映射一個端口，就變得很愚蠢。

```
Quick & Dirty powershell script to use the containter with Docker & WSL2 containers.
# Run this wherever you want the folder created and mapped to.

# create a folder in the current directory
cd (mkdir -ea 0 code-server)

# create the '.config' and 'project' folders in the current location.
mkdir -ea 0 .config,project

# reformat the current folder as a unix-style path 
$LOC = "/$(((pwd).path).substring(0,1).tolower()+((pwd).path).substring(1))" -replace '\\' ,'/' -replace ':',''

# Run the docker image, map it back to port 8080 on the host, and map the .config and project folders  
docker run -it --name code-server -p 127.0.0.1:8080:8080 -v "$LOC/.config:/home/coder/.config"  -v "$LOC/project:/home/coder/project"  codercom/code-server:latest -e PASSWORD=Hi-Mom!
```

## open browser in : localhost:8080
