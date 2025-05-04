# 项目说明

本仓库基于树莓派、Ydlidar Tmini Pro 激光雷达实现二维地图扫描、建图。

二维激光雷达必须要在一个固定的平面做平移旋转运动，才可能绘制出精准的地图。

## 安装与运行

### 环境要求

- Ubuntu 22.04 LTS
- Ros2 humble 桌面版

### 运行步骤

```bash
git clone https://github.com/wapitil/drone_ws.git ~/Projects/drone_ws

# Step 1:安装TminiPro雷达驱动
git clone https://github.com/YDLIDAR/YDLidar-SDK.git 
cd ./YDLidar-SDK-master
mkdir build 
cd ./build 
cmake .. 
make 
sudo make install

# Step 2: 编译src文件并添加工作路径
cd ~/Projects/drone_ws
colcon build --symlink-install --cmake-clean-cache
sudo nano ~/.bashrc
# 在文件末尾添加
source ~/Projects/drone_ws/install/setup.bash --extend

# Step 3: 绑定雷达端口
cd ~/Projects/drone_ws
sudo chmod 777 src/ydlidar_ros2_driver-humble/startup/*
sudo sh src/ydlidar_ros2_driver-humble/startup/initenv.sh
# 然后重新拔插雷达串口，终端输入 
ll /dev/ydlidar

# Step 4 扫图：需要开三个新终端
# 第一个终端 Start radar
ros2 launch ydlidar_ros2_driver ydlidar_launch.py 
# 第二个终端 Release static odom conversion 
ros2 launch rf2o_laser_odometry rf2o_laser_odometry.launch.py 
# 第三个终端  Start gmapping mapping
ros2 launch slam_gmapping slam_gmapping.launch.py

# Step 5：保存打开的图像
sudo apt install ros-$ROS_DISTRO-nav2-map-server
ros2 run nav2_map_server map_saver_cli -t map -f ~Projects/drone_ws/map

```

当Rviz第一次打开时，点击左下角Add By topic ->添加map 添加LaserScan 即可看见扫描的图像。

