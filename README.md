
# ROS2 Isaac Sim Devcontainer with Hokuyo Robot

## Overview
This repository provides a **ready-to-use Docker-based Isaac Sim + ROS 2 Humble environment** for robotics development and simulation.

Includes **Isaac Sim 4.5.0** inside the container (no local installation needed).  
ROS 2 Humble preinstalled and integrated.  
Ready to clone, build, and simulate the **Hokuyo-equipped robot in Isaac Sim** directly.

---

## Prerequisites on Host

**Operating System:** Linux with NVIDIA GPU (or WSL2 with GPU passthrough).  
**NVIDIA GPU drivers:** Installed and compatible with Isaac Sim 4.5.0 (Driver >= 525, CUDA >= 12.x).  
**Docker:** Installed ([Install Docker](https://docs.docker.com/get-docker/)).  
**NVIDIA Container Toolkit:** Installed for GPU passthrough inside containers.
- Install via:
  ```bash
  sudo apt-get install -y nvidia-container-toolkit
  sudo systemctl restart docker
  ```
Optional: X11/VNC server if you want GUI access from the container.

---

## Building the Container

1️⃣ Clone the repository:
```bash
git@github.com:HarunTeper/ROS2-Isaac-Sim-Devcontainer.git
```

2️⃣ Build the container:
```bash
docker build -t isaac-sim-ros2-dev .
```

---

## Running the Container

Run the container with GPU and display support:

```bash
docker run -it --rm \
    --gpus all \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $(pwd):/home/ubuntu/workspace \
    isaac-sim-ros2-dev
```

This will:
- Launch the container with your current workspace mounted.
- Allow Isaac Sim GUI to appear on your host.
- Enable ROS 2 inside the container.

**Note:** If using WSL2 or SSH remote workflows, use VNC or TCP-based X11 forwarding.

---

## ROS 2 Usage Inside the Container

Inside the container, ROS 2 Humble is already installed and ready:

- Source ROS 2 environment:
  ```bash
  source /opt/ros/humble/setup.bash
  ```
- Use ROS 2 CLI tools, `colcon` for building packages, and integrate with Isaac Sim.

---

## Launching Isaac Sim

Inside the container:
```bash
cd /isaac-sim
./isaac-sim.sh
```

Isaac Sim will launch with GPU acceleration inside the container.

---

## Opening the Hokuyo Robot in Isaac Sim

Once Isaac Sim is open:

1️⃣ Go to:
```
File -> Open
```

2️⃣ Navigate to:
```
/home/ubuntu/workspace/hokuyo/hokuyo.usd
```

3️⃣ Click **Open**.

The **Hokuyo-equipped robot will load fully inside Isaac Sim**, including:
- LiDAR sensors.
- Cameras.
- Action Graph.

You can now:
- Simulate navigation and sensor streaming.
- Use the Hokuyo robot inside your Isaac Sim + ROS 2 workflows.

---

## Notes
- If you need to rebuild Isaac Sim cache, it will initialize on the first launch.
- For headless training workflows, you can adapt your `docker run` with Isaac Sim in headless mode.
- For ROS 2 + Isaac Sim integration, ensure your ROS Domain ID matches between your tools and Isaac Sim.

---

## Ready to simulate!
You now have a **portable Isaac Sim + ROS 2 environment** fully integrated with the Hokuyo robot for fast robotics development, testing, and simulation workflows without local clutter.

If you encounter issues or need enhancements (WebRTC streaming, headless deployment scripts), feel free to open an issue or contact the maintainer.
