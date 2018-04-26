# droneci
drone ci for DevOps

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
${DRONE_HOST} =>  ip in which install drone docker container
8083 => drone's external port
${DRONE_SECRET} => generate a codes put in drone-server and drone-agent. let drone-server connect to drone-agent.
${GITLAB_URL} => gitlab url
${DRONE_GITHUB_CLIENT} and ${DRONE_GITHUB_SECRET} => just copy gitlab's oauth return codes
```
version: '2'

services:
  drone-server:
    image: drone/drone:0.8

    ports:
      - 8083:8000
      - 9000
    volumes:
      - /var/lib/drone:/var/lib/drone/
    restart: always
    environment:
      - DRONE_OPEN=true
      - DRONE_HOST=http://${DRONE_HOST}:8083
      #- DRONE_GITHUB=true
      #- DRONE_GITHUB_CLIENT=${DRONE_GITHUB_CLIENT}
      #- DRONE_GITHUB_SECRET=${DRONE_GITHUB_SECRET}
      - DRONE_GITLAB=true
      - DRONE_GITLAB_CLIENT=${DRONE_GITLAB_CLIENT}
      - DRONE_GITLAB_SECRET=${DRONE_GITLAB_SECRET}
      - DRONE_GITLAB_URL=http://${GITLAB_URL}
      - DRONE_SECRET=${DRONE_SECRET}

  drone-agent:
    image: drone/agent:0.8

    command: agent
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DRONE_SERVER=drone-server:9000
      - DRONE_SECRET=${DRONE_SECRET}

