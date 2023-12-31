<img src="https://jenkins.io/sites/default/files/jenkins_logo.png"/>
General installation of a CI/CD system based on Jenkins.

| Plug-in         | Link                                        | Version                           | Release Notes                                        |
| :-------------- | :------------------------------------------ | :-------------------------------- | :--------------------------------------------------- |
| Blue Ocean      | https://plugins.jenkins.io/blueocean/       | blueocean:1.27.9                  | https://plugins.jenkins.io/blueocean/releases/       |
| Docker Pipeline | https://plugins.jenkins.io/docker-workflow/ | docker-workflow:563.vd5d2e5c4007f | https://plugins.jenkins.io/docker-workflow/releases/ |
  
# Installation
## Build the Jenkins Docker Image
```
docker build -t cicd:1.0 .
```

## Create the network 'jenkins'
docker network create jenkins
```
Check docker network by
```
docker network ls
```

## Run the Container
### <img src="https://www.freepnglogos.com/uploads/apple-logo-png/file-apple-logo-black-svg-wikimedia-commons-1.png" width="20"/> MacOS / <img src="https://www.freepnglogos.com/uploads/linux-png/linux-file-tux-enhanced-svg-wikimedia-commons-9.png" width="20"/> Linux
```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 \
  cicd:1.0
```
Check if docker service is running
```
docker ps
```

### <img src="https://www.freepnglogos.com/uploads/windows-logo-png/windows-logo-windows-symbol-meaning-history-and-evolution-4.png" width="20"/> Windows
```
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 cicd:1.0
```
Check if docker service is running
```
docker ps
```

## Get the Password
```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## Connect to the Jenkins
```
https://localhost:8080/
```

## Log into Jenkins Container
```
docker exec -it jenkins-blueocean bash
```
Workspaces
```
cd /var/jenkins_home/workspace
ls -ltra
```


## Installation Reference
https://www.jenkins.io/doc/book/installing/docker/


## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine

https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
```
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
docker inspect <container_id> | grep IPAddress
```
