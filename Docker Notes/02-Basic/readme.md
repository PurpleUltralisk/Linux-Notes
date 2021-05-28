# Basic Commands

## Docker Run
`docker run`
`docker run --name [name] [container]`
`docker run [container]:[version #]`

## Docker ps
List all running containers 
`docker ps -a` shows recently stopped containers

## Docker stop
Stop a container, here we must provide a container name
`docker stop [name]`

To remove the container so that it doesn't show in `docker ps -a` command: 
`docker rm [name]`

To get rid of the image as well
`docker images` to see a list of images
`docker rmi [image name]`
be sure to remove all dependent containers on this image

## Pull Image
`$ docker pull [image name]`

**Note:** 
Docker is used as a service/process, container only lives for as long as the process inside is running.
This is why if we just run an Ubuntu image, the container will exit right away. 

To work with an Ubuntu image, we can append an option like `sleep 5`
`docker run ubuntu sleep 5`

## Execute Command in Container
`docker exec [container name] [command]`

## Detached mode
Container will continue to run, but you retain the command prompt
`docker run -d [app name]`

To re-attach to a container
`docker attach [container name/id]`

## Inspect Container
`docker inspect [container]`

## Container Logs
`docker logs [container]`



