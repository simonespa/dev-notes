# Docker

- [List containers](#list-containers)
- [List containers and images](#list-containers-and-images)
- [Remove all containers](#remove-all-containers)
- [Remove all images](#remove-all-images)
- [Networks](#networks)

## List containers

```
docker ps -a
```

## List containers and images

```
docker container ls -a
docker image ls -a
```

## Remove all containers

```
docker stop $(docker container ls -a -q)
docker rm $(docker container ls -a -q)
```

## Remove all images

```
docker rmi $(docker image ls -a -q) -f
```

## Networks

```
docker network ls
docker network inspect sounds
docker network rm sounds
docker network create --driver bridge sounds
```
