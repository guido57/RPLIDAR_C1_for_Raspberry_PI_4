# RPLIDAR_C1_for_Raspberry_PI_4
Build the RPLIDAR C1 package for ROS2 in an Ubuntu Jammy arm64v8 docker

# OVERVIEW 

The goal of this repository is to obtain a RPLIDAR C1 Package for ROS2 to be simply copied to your Raspberry PI 4.

### Why? Because your Raspberry PI 4 1GB RAM (as mine) hasn't enough memory to build the package directly. 

Therefore with Docker you build the package in a much more performing environment. For instance:
* CPU: Intel I5/I9 or AMD Ryzen 5/7/9 or anyway the best PC you have
* OS:Windows 10 or Windows 11 or Ubuntu or other Linux distributions that can run Docker
* Docker container with Ubuntu 22.04 and platform ARM64V8


# INSTRUCTIONS

## 1) Install Docker in your performing PC 
https://docs.docker.com/engine/install/

## 2) Download the file "arm64_dockerfile"

  ```
  git clone https://github.com/guido57/RPLIDAR_C1_for_Raspberry_PI_4
  ```
## 3) Modify the environment constant RPI4_DIR, at line 55, with the destination directory where you want to place this package in the Raspberry PI

```
ENV RPI4_DIR=/home/robot/RPLIDAR_C1_ws/
```

## 4) Install the qemu for docker to enable cross platform emulation as you most likely have an amd64 / x86_64 in your performing PC

```
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```
 
## 5)  Build the container ros2-humble-arm64 for an arm64v8 platform reading the dockerfile arm64_dockerfile

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
## 6) For testing, you can optionally run the just created docker image
   ```
   docker run -it --network host --platform linux/arm64 ros2-humble-arm64 /bin/bash
   ```
When the prompt appears, as here:

```
root@guido-Neptune-series-i9:/home/robot/RPLIDAR_C1_ws#
```

it means you are running your arm64v8 docker image inside you amd64 machine.

## 7) Copy your RPLIDAR_C1_ws package
   The only thing you are left to do is to copy your package to the real place where you'll us it, i.e. the directory /home/robot/RPLIDAR_C1_ws in your Raspberry PI 4. 

   To do this, you can, for instance, launch an scp command to you running image

   ```
   docker run -it --network host --platform linux/arm64 ros2-humble-arm64 scp -r /home/robot/RPLIDAR_C1_ws/ robot@192.168.1.63:/home/robot/
   ``` 

   you'll be prompted for the password of the user robot in your Raspberry PI 4.

## 8) And now? See a working field test in 

https://hackaday.io/project/197642-rplidar-c1-with-raspberry-pi-4-and-ros2
