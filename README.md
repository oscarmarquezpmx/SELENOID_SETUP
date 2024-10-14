# SELENOID_SETUP
Selenoid Setup Example Project

Based on the provided files (`LINUX_SETUP.txt`, `README.md`, and `Windows_Setup.txt`), I'll create a comprehensive README file for this project.

**Selenoid Setup Example Project**
====================================

A step-by-step guide to setting up Selenoid, a container-based browser testing framework, on both Linux and Windows platforms.

Table of Contents
-----------------

1. [Prerequisites](#prerequisites)
2. [Setup Instructions](#setup-instructions)
	* [Linux Setup](#linux-setup)
	* [Windows Setup](#windows-setup)
3. [Additional Resources](#additional-resources)

### Prerequisites

* Docker installed on your system (follow the official installation instructions for [Linux](https://docs.docker.com/engine/install/linux/) and [Windows](https://docs.docker.com/docker-for-windows/))
* Selenoid images pulled from the official Docker Hub repository (`aerokube/selenoid:latest-release` and `selenoid/video-recorder:latest-release`)
* A directory to store Selenoid configuration files (`config`) and video recordings (`videos`)

### Setup Instructions

#### Linux Setup

1. Create a separate IP network for your Selenoid containers using the following command:
```bash
docker network create --driver=bridge --subnet=192.168.0.0/16 selenoid
```
2. Pull the required images:
```bash
docker pull selenoid/vnc_chrome:124.0
docker pull selenoid/video-recorder:latest-release
```
3. Run the Selenoid container with the following command (make sure to update the `TZ` environment variable as needed):
```bash
docker run -d --name selenoid \
  -p 4444:4444 \
  -e TZ=America/Mexico_City \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /home/oscar/Downloads/selenoid/config/:/etc/selenoid/:ro \
  -v /home/oscar/Downloads/selenoid/videos:/opt/selenoid/video/ \
  -e OVERRIDE_VIDEO_OUTPUT_DIR=/home/oscar/Downloads/selenoid/videos \
  aerokube/selenoid:latest-release \
  -limit 8
```
4. Run the Selenoid UI container with one of the following commands (choose either the default bridge network or specify a custom network):
```bash
docker run -d --name selenoid-ui -p 8081:8080 aerokube/selenoid-ui:latest-release --selenoid-uri=http://selenoid:4444

# or, using a custom network
docker run -d --name selenoid-ui --network selenoid -p 8081:8080 aerokube/selenoid-ui:latest-release --selenoid-uri=http://selenoid:4444
```
#### Windows Setup

1. Pull the required images:
```bash
docker pull selenoid/vnc_chrome:124.0
docker pull selenoid/video-recorder:latest-release
```
2. Create a Docker volume for storing video recordings:
```bash
docker volume create selenoid-videos
```
3. Run the Selenoid container with one of the following commands (choose either the default bridge network or specify a custom network):
```bash
docker run -d --name selenoid \
  -p 4444:4444 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /c/selenoid/config/:/etc/selenoid/:ro \
  -v /c/selenoid/video/:/opt/selenoid/video/ \
  -e OVERRIDE_VIDEO_OUTPUT_DIR=/c/selenoid/video/ \
  aerokube/selenoid:latest-release \
  -limit 16
```
4. Run the Selenoid UI container with one of the following commands (choose either the default bridge network or specify a custom network):
```bash
docker run -d --name selenoid-ui -p 8081:8080 aerokube/selenoid-ui:latest-release --selenoid-uri=http://selenoid:4444

# or, using a custom network
docker run -d --name selenoid-ui --network bridge -p 8081:8080 aerokube/selenoid-ui:latest-release --selenoid-uri=http://selenoid:4444
```

### Additional Resources

* [Selenoid Documentation](https://aerokube.com/docs/selenoid/)
* [Docker Documentation](https://docs.docker.com/)

This README file should provide a comprehensive guide for setting up Selenoid on both Linux and Windows platforms. If you encounter any issues or have further questions, please refer to the official documentation links provided above.
oscar@AMD:~/Downloads/selenoid$ ls -R
.:
config  videos

./config:
browsers.json

./videos: