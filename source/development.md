# diyHue - Development

diyHue is an application written in python 3. To do any development you'll need some basic programming knowledge.

First you create a fork of diyhue on github. [Create diyhue fork](https://github.com/diyHue/diyHue/fork).

If you already had a fork, make sure it's in sync with the master branch.

```shell
# Add the remote, call it "upstream": (only once)
git remote add upstream https://github.com/diyHue/diyHue.git

# Fetch all the branches of that remote into remote-tracking branches,
# such as upstream/master:
git fetch upstream

# Make sure that you're on your master branch:
git checkout master

# Rewrite your master branch so that any commits of yours that
# aren't already in upstream/master are replayed on top of that
# other branch:
git rebase upstream/master
```

## Repository content

- `.build/` - build requirements
  - `select.sh` - Script to move the correct coap client and entertainment service for the target platform, used in docker.
- `.github/` - github specific files (workflows / issue templates)
- `BridgeEmulator/` - The actual bridge emulator
  - `functions/` - functions used in the bridge
  - `protocols/` - Light connections (mqtt, esphome, tasmota,...)
  - `web-ui-src/` - The webinterface (unpackaged)
  - `web-ui/` - Webinterface packaged
  - `HueEmulator3.py` - The main executable
  - `coap-client-*` - Pre-compiled [coap client](http://manpages.ubuntu.com/manpages/bionic/man5/coap-client.5.html) for connection with Ikea Tradfri bridge
  - `entertainment-*` - pre-compiled entertainment service. Used to setup the entertainment service for the hue bridge.
  - `ssl_server2_diyhue.c` - Source of entertainment service.
- `RemoteApi/` - Server used to create a remote connection.

## Docker development

If you don't want to install python or the diyhue emulator on your machine, you can use these steps to setup your local development environment with the help of docker. Docker is the recommended way to develop and test your changes to diyhue.

### Requirements

- Docker (desktop), windows Docker desktop with linux containers
- Editor like [VSCode](https://code.visualstudio.com/)
- A folder to put the configuration
- You cannot have the bridge running on this PC, because of the required ports

### Build docker image

The docker image for diyHue can be build for multiple architectures. Recently we added a Dockerfile that works for all (supported) architectures.

- Linux 64-bit `amd64`
- Arm (raspberry pi 3 eg.) `arm/v7`
- Arm 64-bit (raspberry pi 3b/4 eg.) `arm64`

Run the following command from the root of the repository to build the image

```shell
docker build -t diyhue:development -f ./.build/Dockerfile .

# Or the following if you enabled buildx for multi architecture images
docker build -t diyhue:development -f ./.build/Dockerfile --load .
```

### Start docker image

Once you made your changes and build the image, you'll need to run the following command to run your new image in debug mode.
Before running change the values to your needs, so make sure you set the `MAC` variable to your used network card and the `IP` to the IP of your machine.
Also change the folder `/Users/you/diyhue/export` to your own configuration folder.

```shell
# Stop and remove current dev container
docker stop diyhue-dev && docker rm diyhue-dev

# Create new container
docker run -d --name "diyhue-dev" --restart="unless-stopped" --network="bridge" \
    -v '/Users/you/diyhue/export':'/opt/hue-emulator/export/':'rw' \
    -e MAC='f4:0f:05:01:01:01' \
    -e IP='192.168.x.x' \
    -e disable-online-discover='true' \
    -e DEBUG='true' \
    -p 80:80/tcp -p 443:443/tcp -p 1900:1900/udp -p 2100:2100/udp -p 1982:1982/udp \
    diyhue:development

# Open logging
docker logs -f diyhue-dev
```

## Publishing the docker image

If you want to publish the multiarchitecture image of diyHue, you'll need to setup [Docker buildx](https://docs.docker.com/buildx/working-with-buildx/). Once setup it's a one line command to publish this image to the docker hub for multiple architectures.

Caution, the first time it has to download a lot of packages for 3 platforms. This will take some time. After the first time the requirements are cached and building should be a lot faster.

```shell
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t diyhue/core:development -f .build/Dockerfile --push .
```
