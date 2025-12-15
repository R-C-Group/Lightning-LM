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

* 安装依赖及编译

```bash
bash ./scripts/install_dep.sh

# 解压Pangolin

colcon build

# 注意要安装colcon包
sudo apt install python3-colcon-common-extensions

#最后要记得source一下
# source ~/colcon_ws/install/local_setup.bash
echo "source ~/colcon_ws/install/local_setup.bash" >> ~/.bashrc
```
