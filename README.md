# droneci
drone ci for DevOps

#### before install:
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

