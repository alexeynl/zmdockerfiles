# zmdockerfiles
I don't find any zoneminder docker based on fresh LTS Ubuntu 22.04 Jammy Jellyfish. So i built it for myself.

This image is based on official dockerfile https://github.com/ZoneMinder/zmdockerfiles/tree/master/release/ubuntu20.04 and fixed some problems i had encoutered.

## Usage

**Note:** Detailled usage instructions for the development and release Dockerfiles are contained within each Dockerfile.

Docker image is published to Docker Hub and can be pulled directly from there 
https://hub.docker.com/repository/docker/alexeynl/zoneminder/general

### Docker command

```bash
docker run -d -t -p 7878:80 \
    -e TZ='Europe/Moscow' \
    -v ~/zoneminder/events:/var/cache/zoneminder/events \
    -v ~/zoneminder/images:/var/cache/zoneminder/images \
    -v ~/zoneminder/mysql:/var/lib/mysql \
    -v ~/zoneminder/logs:/var/log/zm \
    --shm-size="512m" \
    --name zoneminder \
    alexeynl/zoneminder:latest
```

Example for passing a host device to the container (for hardware acceleration via DecoderHWAccelName/DecoderHWAccelDevice):

```bash
docker run [...] \
    [...]
    --device /dev/dri \
    [...]
```
### Docker compose file
docker-compose.yml
```
version: '3.1'
services:
    zoneminder:
        container_name: zoneminder
        image: alexeynl/zoneminder:latest
        restart: unless-stopped
        ports:
            - 7878:80
        #network_mode: "bridge" #optional
        privileged: true
        shm_size: 512M
        environment:
            - TZ=${TZ:-Europe/Moscow}
        devices:
            - /dev/dri:/dev/dri
        volumes:
            - config:/config
            - events:/var/cache/zoneminder/events
            - images:/var/cache/zoneminder/images
            - mysql:/var/lib/mysql
            - logs:/var/log/zoneminder
volumes:
  config:
  events:
  images:
  mysql:
  logs:
```
