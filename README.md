# ros_laboratory
## Instruction to build everything

### Requirements
 * Ubuntu 18.04
 * ROS melodic
 * ROS2 dashing

install catkin module (needed for build of ros melodic things)
```
  sudo apt-get install ros-melodic-catkin python-catkin-tools
```

install gtsam (follow https://github.com/eperdices/LeGO-LOAM-SR)
```
wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
cd ~/Downloads/gtsam-4.0.0-alpha2/
mkdir build && cd build
cmake ..
sudo make install
```

### Terminal 1 (if you want to try lego_loam, for ros1 - optional)
```
cd dev_ws
source /opt/ros/melodic/setup.bash
catkin build
```
> I obtained some warnings that can be easily ignored

### Terminal 2
install colcon module
```
sudo sh -c 'echo "deb [arch=amd64,arm64] http://repo.ros2.org/ubuntu/main `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt update
sudo apt install python3-colcon-common-extensions
```
build lego_loam_sr
```
cd dev_ws2
source /opt/ros/dashing/setup.bash
catkin build
```

### Terminal 3
follow https://industrial-training-master.readthedocs.io/en/melodic/_source/session7/ROS1-ROS2-bridge.html
```
cd ros1_bridge_ws/
source /opt/ros/melodic/setup.bash
source /opt/ros/dashing/setup.bash

source /dev_ws/devel/setup.bash
source /dev_ws2/install/setup.bash

colcon build --packages-select ros1_bridge --cmake-force-configure --cmake-args -DBUILD_TESTING=FALSE
```
> this build will take long (~ 13 minutes on my pc)


## RUN things

### Terminal 1
```
cd dev_ws/
source /opt/ros/melodic/setup.bash
roscore &
rosbag play *.bag --clock --topic /velodyne_points
```
> I used, for example, 2017-06-08-15-49-45_0.bag, downloaded from https://drive.google.com/drive/folders/1_t5fX5yIqY-y6sAifY8pVWX4O9LCK5R2?usp=sharing

### Terminal 2
```
cd dev_ws2/
source dev_ws2/src/install/setup.bash
ros2 launch lego_loam_sr run.launch.py
```
### Terminal 3

```
cd ros1_bridge_ws/
source bridgeinstall/local_setup.bash
ros2 run ros1_bridge dynamic_bridge --print-pairs
```
