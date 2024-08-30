# RPLIDAR_C1_for_Raspberry_PI_4
Build the RPLIDAR C1 package for ROS2 in an Ubuntu Jammy arm64v8 docker

# OVERVIEW 

The goal of this repository is to obtain a RPLIDAR C1 Package built in an ARM64V8 platform (the same of Raspberry PI 4 with Ubuntu Jammy 64 bits) but executed in a much more performing environment. For instance:
* CPU: Intel I5/I9 or AMD Ryzen 5/7/9 or anyway the best PC you have
* OS:Windows 10 or Windoqs 11 or Ubuntu or other Linux distributions that can run Docker
* Docker container with Ubuntu 22.04 

# INSTRUCTIONS

1) Install docker in your performing PC https://docs.docker.com/engine/install/

2) Download the file "arm64_dockerfile"

  ```
  git clone https://github.com/guido57/RPLIDAR_C1_for_Raspberry_PI_4
  ```
3) Modify arm64_dockerfile setting, at line 55, the destination directory where you want to place this package in the Raspberry PI

```
ENV RPI4_DIR=/home/robot/RPLIDAR_C1_ws/
```

4) Install the qemu for docker to enable cross platform emulation as you most likely have an amd64 / x86_64 in your performing PC

```
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```
 
5)  Build the container ros2-humble-arm64 for an arm64v8 platform reading the dockerfile arm64_dockerfile

  ```
  docker build -t ros2-humble-arm64 --platform linux/arm64 -f  arm64_dockerfile .
  ```

 This step can take a few minutes (Around 10 minutes on my Intel I9 12th gen).

 You can check that the ros2-humble-arm64 image really exists with 

 ```
 docker image ls
 ```

which should list something like this: 

```
REPOSITORY            TAG      IMAGE ID       CREATED           SIZE
ros2-humble-arm64    latest    b9499245c368   35 seconds ago    980MB
```
6) Run the just created docker image
   ```
   docker run -it --network host --platform linux/arm64 ros2-humble-arm64 /bin/bash
   ```
When the prompt appears, like this:

```
root@guido-Neptune-series-i9:/home/robot/RPLIDAR_C1_ws#
```

it means you have succesfully created the docker image.





