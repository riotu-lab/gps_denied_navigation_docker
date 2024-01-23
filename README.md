# GPS Denied Navigation Docker Image
A Docker image for the development environment of the GPS Denied Navigation project.

What is included in the Docker image is
* Ubuntu 22.04 (jammy)
* ROS 2 Humble
* Gazebo Garden
* Development tools required by PX4 firmware + PX4-Autopilot source code

# Build Docker Image
Docker should be installed before proceeding with the next steps. See docker installation instructions [here](https://docs.docker.com/engine/install/ubuntu/)


* Clone this package `git clone https://github.com/riotu-lab/gps_denied_navigation_docker.git`. You will need a GitHub token. Contact the admins for that.
* **NOTE** If you want to build an image with `CUDA` (recommended) run
    ```bash
    cd gps_denied_navigation_docker/docker
    make px4-simulation-cuda12.2.0-ubuntu22
    ```

This builds a Docker image that has the required PX4-Autopilot source code and the required dependencies, ROS 2 Humble Desktop, and `ros2_ws` that contains [gps_denied_navigation_sim](https://github.com/riotu-lab/gps_denied_navigation_sim.git) package and other ROS 2 packages.

The Gazebo version in the provided Docker image is Gazebo `garden`.

# Run a Docker Container
* Obtain a valid `GIT_TOKEN` from GitHub admins.
* `export GIT_USER=github_user_name && export GIT_TOKEN=git_token_that_you_obtained_from_admins`
* You can add the above exports you your `~/.bashr` script to avoid exporting them in each new terminal.
* Run the following script to run and enter the docker container  
    ```bash
    cd gps_denied_navigation_docker
    ./docker_run.sh
    ```
    This will run a docker container named `gpsdnav` 

* Once the above script is executed successfully, you should be in the docker container terminal. The docker defualt name is `gpsdnav`. The username inside the docker and its password is `user`


# Build ros2_ws
Enter the docker container, and execute the following
```bash
cd ~/shared_volume
# Execute the following step, if you have not created the ros2_ws already
mkdir -p ros2_ws/src
cd ros2_ws/src
git clone https://github.com/riotu-lab/gps_denied_navigation_sim.git
cd gps_denied_navigation_sim
./install.sh
```

The `install.sh` script clones and builds the `PX4-Autopilot` in the `~/shared_volume `. It also clones necessary drone models inside the corresponding directories in `PX4-Autopilot` sub-directories.

# PX4 Autopilot & ROS 2 Wrokspace
* You can find the `ros2_ws` and the `PX4-Autopilot` inside the shared volume. The path of shared volume inside the container is `/home/user/shared_volume`. The path of the shared volume outside the container is `$HOME/gpsdnav_shared_volume`.

# Run Simulation
* Follow the instructions of the [gps_denied_navigation_sim](https://github.com/riotu-lab/gps_denied_navigation_sim.git) package to run the simulation.
* You can re-enter the docker container by executing the `docker_run.sh`.