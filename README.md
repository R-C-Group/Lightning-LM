<div align="center">
<h1>代码学习——Lightning-Speed Lidar Localization and Mapping</h1>
</div>


# 安装配置

* 采用ubuntu22.01(ROS2 humble)
* 首先创建一个工作空间

```bash
mkdir -p colcon_ws/src
cd ~/colcon_ws/src

git clone https://github.com/gaoxiang12/lightning-lm.git
# 或者采用我的代码
git clone git@github.com:R-C-Group/Lightning-LM.git
```

* 安装依赖（包含：Pangolin）

```bash
bash ./scripts/install_dep.sh
sudo apt-get install libunwind-dev
sudo apt-get install libgoogle-glog-dev

# 解压Pangolin，然后进入到对应目录
cd /lightning-lm/thirdparty/Pangolin-0.9.3/
./scripts/install_prerequisites.sh

# Configure and build
sudo apt-get install python3-wheel #可能缺少
cmake -B build
cmake --build build

cd build
sudo make install #必须
```

* 编译

```bash
colcon build

# 注意要安装colcon包
sudo apt install python3-colcon-common-extensions

#最后要记得source一下
# source ~/colcon_ws/install/local_setup.bash
echo "source ~/colcon_ws/install/local_setup.bash" >> ~/.bashrc
```


* 数据准备是需要将数据转换为ros2的db3格式的，不过作者提供了转换后的数据：BaiduYun: https://pan.baidu.com/s/1XmFitUtnkKa2d0YtWquQXw?pwd=xehn 提取码: xehn


# 实验测试


### 建图测试

1. 实时建图（实时播包）
    - 启动建图程序:
      ```ros2 run lightning run_slam_online --config ./config/default_nclt.yaml```
    - 播放数据包
    - 保存地图 ```ros2 service call /lightning/save_map lightning/srv/SaveMap "{map_id: new_map}"```
2. 离线建图（遍历跑数据，更快一些）
    - ```ros2 run lightning run_slam_offline --config ./config/default_nclt.yaml --input_bag 数据包```
    - 结束后会自动保存至data/new_map目录下
3. 查看地图
    - 查看完整地图：```pcl_viewer ./data/new_map/global.pcd```
    - 实际地图是分块存储的，global.pcd仅用于显示结果
    - map.pgm存储了2D栅格地图信息
    - 请注意，在定位程序运行过程中或退出时，也可能在同目录存储动态图层的结果，所以文件可能会有更多。

### 定位测试

1. 实时定位
    - 将地图路径写到yaml中的 system-map_path 下，默认是new_map（和建图默认一致)
    - 将车放在建图起点处
    - 启动定位程序：
      ```ros2 run lightning run_loc_online --config ./config/default_nclt.yaml```
    - 播包或者输入传感器数据即可

2. 离线定位
    - ```ros2 run lightning run_loc_offline --config ./config/default_nclt.yaml --input_bag 数据包```

3. 接收定位结果
    - 定位程序输出与IMU同频的TF话题（50-100Hz）
