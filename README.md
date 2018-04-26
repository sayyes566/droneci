# droneci

### drone ci with GitLab

#### registry oauth to allow Drone login to gitlab :
```
into gitlab > profile > settings > applications
Name: Drone
Redirect Url: http://${drone_host}:8083/authorize
Scops: all checked
```
####  install drone and access gitlab repositories

```
$ docker-compose up -d 
```
#### in docker-compose.yml

```
${DRONE_HOST} =>  ip in which install drone docker container.
8083 => drone's external port. custome by your favorite number.
${DRONE_SECRET} => generate a codes put in drone-server and drone-agent. let drone-server connect to drone-agent.
${GITLAB_URL} => gitlab url.
${DRONE_GITHUB_CLIENT} and ${DRONE_GITHUB_SECRET} => just copy gitlab's oauth return codes.
```

