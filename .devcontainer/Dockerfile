ARG VARIANT=ubuntu-22.04
FROM nvcr.io/nvidia/isaac-sim:4.5.0

# --- Configure environment ---
ENV DEBIAN_FRONTEND=noninteractive
ENV ACCEPT_EULA=Y
ENV PRIVACY_CONSENT=Y
ENV LANG=en_US.UTF-8

# --- Set up user and sudo ---
WORKDIR /home/ubuntu/workspace

RUN echo "ubuntu:ubuntu" | chpasswd && \
    apt-get update && apt-get install -y sudo && \
    usermod -aG sudo ubuntu

# --- Locale setup for ROS ---
RUN apt-get update && apt-get install -y locales && \
    locale-gen en_US.UTF-8 && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

# --- Add ROS2 repository ---
RUN apt-get update && \
    apt-get install -y software-properties-common curl git && \
    add-apt-repository universe && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# --- System upgrade + fix dependencies + install ROS ---
    RUN rm -f /etc/apt/sources.list.d/*sury* && \
    apt-get update --allow-releaseinfo-change && apt-get upgrade -y && \
    apt-get install -y libbrotli1=1.0.9-2build6 --allow-downgrades && \
    apt-get install -y \
        libfreetype6-dev \
        libfontconfig1-dev \
        ros-humble-desktop
    
RUN apt-get update && apt-get install -y ros-humble-ackermann-msgs

# --- ROS2 development tools ---
RUN apt-get install -y python3-pip python3-colcon-common-extensions && \
    python3 -m pip install -U colcon-ros

# --- Entrypoint ---
USER ubuntu

RUN echo '. /opt/ros/humble/setup.sh' >> /home/ubuntu/.bashrc

ENV DISPLAY=unix:1
ENV ROS_LOCALHOST_ONLY=0
ENV ROS_DOMAIN_ID=178

SHELL ["/bin/bash", "-c"]
