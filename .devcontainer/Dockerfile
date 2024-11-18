ARG VARIANT=ubuntu-22.04
FROM nvcr.io/nvidia/isaac-sim:4.2.0

# --- Configure environment ---

# Set up environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV ACCEPT_EULA=Y
ENV PRIVACY_CONSENT=Y

# --- Set up user and sudo ---

# set up work directory
WORKDIR /home/ubuntu/workspace

# Set sudo password for ubuntu user
RUN echo "ubuntu:ubuntu" | chpasswd

RUN apt-get update && apt-get install -y sudo

# Add user ubuntu to sudoers group
RUN usermod -aG sudo ubuntu

# --- Install ROS2 ---

# install locales for UTF-8 support
RUN apt-get update && apt-get install -y locales && \
    locale-gen en_US.UTF-8 && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

# set locale environment variables
ENV LANG=en_US.UTF-8

# add ROS2 repository
RUN apt-get update && \
    apt-get install -y software-properties-common curl git && \
    add-apt-repository universe && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# install ROS2
RUN apt-get update && \
    apt-get install -y ros-humble-desktop

# install ROS2 development tools
RUN apt-get -y install python3-pip python3-colcon-common-extensions

# --- Entrypoint ---

# set user
USER ubuntu

# source ROS2 setup file
RUN echo '. /opt/ros/humble/setup.sh' >> /home/ubuntu/.bashrc

# configure environment variables
ENV DISPLAY=unix:1
ENV ROS_LOCALHOST_ONLY=0
ENV ROS_DOMAIN_ID=0


# --- Set up entrypoint ---
SHELL ["/bin/bash", "-c"]