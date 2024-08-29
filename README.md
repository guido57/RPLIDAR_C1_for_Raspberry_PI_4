# RPLIDAR_C1_for_Raspberry_PI_4
Build the RPLIDAR C1 package for ROS2 in an Ubuntu Jammy arm64v8 docker

# OVERVIEW 

The goal of this repository is to obtain a RPLIDAR C1 Package built in an ARM64V8 platform (the same of Raspberry PI 4 with Ubuntu Jammy 64 bits) but executed in a much more performing environment:
* CPU: Intel I5/I9 or AMD Ryzen 5/7/9 or anyway the best PC you have
* OS:Windows 10 or Windoqs 11 or Ubuntu or other Linux distributions that can run Docker
* Docker container with Ubuntu 22.04 

# INSTRUCTIONS

1) Install docker in your performing PC https://docs.docker.com/engine/install/

2) Create the following file "arm64_dockerfile" in a new directory of your choice

  ```
  FROM arm64v8/ubuntu:jammy
  
  # update repositories only once
  RUN apt-get update
  
  # Install necessary dependencies
  RUN apt-get install -y \
     build-essential \
     cmake \
     git \
     python3-pip \
     wget \
     curl \
     lsb-release \
     gnupg2 \
     locales
  
  
  # add locale     
  RUN locale-gen en_US en_US.UTF-8 && \
     update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
  ENV LANG en_US.UTF-8
  
  RUN apt-get install -y software-properties-common
  
  # Add ROS 2 repository
  RUN add-apt-repository -y universe
  RUN apt-get update
  
  #Download the ros.key    
  RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
  
  RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
  
  #install tzdata without interaction to avoid interaction when installing ros-dev-tools
  RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata
  
  RUN apt-get update
  
  # Install ROS 2 Humble
  RUN apt-get install -y \
     ros-humble-ros-base \
     ros-dev-tools
  
  # Source ROS 2 setup
  SHELL ["/bin/bash", "-c"]
  RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
  
  
```

3)  Build the container ros2-humble-arm64 for an arm64v8 platform reading the dockerfile arm64_dockerfile

  ```
  docker build -t ros2-humble-arm64 --platform linux/arm64 -f  arm64_dockerfile .
  ```

 The output image will be ros2-humble-arm64

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
 
4) Install the qemu for docker to enable cross platform emulation as you most likely have an amd64 / x86_64 in your performing PC

```
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```
 

...  other steps ...
