# 双足机器人强化学习运动学习项目 / Bipedal Robot RL Locomotion Learning Project

[![IsaacSim](https://img.shields.io/badge/IsaacSim-4.2.0-silver.svg)](https://docs.omniverse.nvidia.com/isaacsim/latest/overview.html)
[![Isaac Lab](https://img.shields.io/badge/IsaacLab-1.2.0-silver)](https://isaac-sim.github.io/IsaacLab)
[![Python](https://img.shields.io/badge/python-3.10-blue.svg)](https://docs.python.org/3/whatsnew/3.10.html)
[![Linux platform](https://img.shields.io/badge/platform-linux--64-orange.svg)](https://releases.ubuntu.com/20.04/)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/license/mit)

## 概述 / Overview

本项目基于 [Isaac Lab](https://github.com/isaac-sim/IsaacLab) 框架，专注于双足机器人（如 [LimX Dynamics TRON 1](https://www.limxdynamics.com/en/tron1)）的强化学习训练与仿真。
通过本项目，您可以训练双足机器人在多种复杂环境中通过，包括平地、崎岖地形和楼梯等。

This repository is designed for training and simulating bipedal robots, specifically the [LimX Dynamics TRON 1](https://www.limxdynamics.com/en/tron1).
Leveraging the [Isaac Lab](https://github.com/isaac-sim/IsaacLab) framework, it enables the training of bipedal locomotion policies across various environments, including flat ground, rough terrain, and stairs.

**关键词 / Keywords:** isaaclab, locomotion, bipedal, pointfoot, TRON1, reinforcement learning

---

## 安装指南 / Installation

### 1. 预备工作 / Prerequisites

请确保您的系统已安装 **Isaac Sim** 和 **Isaac Lab**。
Please ensure **Isaac Sim** and **Isaac Lab** are installed on your system.

- **官方安装 / Official Installation**:
  参考 [Isaac Lab 安装指南](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/index.html)。推荐使用 Conda 环境管理。
  Follow the [installation guide](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/index.html). Using Conda is recommended.

- **非官方一键安装 (Ubuntu 20.04/22.04)**:
  支持 Isaac Sim v1.4.1 至 v4.x.x。
  ```bash
  wget -O install_isaaclab.sh [https://docs.robotsfan.com/install_isaaclab.sh](https://docs.robotsfan.com/install_isaaclab.sh) && bash install_isaaclab.sh


- 将仓库克隆到Isaac Lab安装目录之外的独立位置（即在`IsaacLab`目录外）：

  Clone the repository separately from the Isaac Lab installation (i.e. outside the `IsaacLab` directory):

```bash
# 选项 1: HTTPS / Option 1: HTTPS
git clone http://8.141.22.226/Bobbin/limxtron1lab.git

# 选项 2: SSH
git clone git@8.141.22.226:Bobbin/limxtron1lab.git
```

```bash
# Enter the repository
conda activate isaaclab
cd bipedal_locomotion_isaaclab
```

- Using a python interpreter that has Isaac Lab installed, install the library

```bash
python -m pip install -e exts/bipedal_locomotion
```

- 为了使用MLP分支，需要安装该库 / To use the mlp branch, install the library

```bash
cd bipedal_locomotion_isaaclab/rsl_rl
python -m pip install -e .
```

## IDE设置（可选）/ Set up IDE (Optional)

要设置IDE，请按照以下说明操作：
To setup the IDE, please follow these instructions:

- 将.vscode/settings.json中的路径替换成使用者所使用的Isaaclab和python路径，这样当使用者对Isaaclab官方函数或变量进行检索的时候，可以直接跳入配置环境代码的定义。

- Replace the path in .vscode/settings.json with the Isaaclab and python paths used by the user. This way, when the user retrieves the official functions or variables of Isaaclab, they can directly jump into the definition of the configuration environment code.

## 训练双足机器人智能体 / Training the bipedal robot agent

- 使用`scripts/rsl_rl/train.py`脚本直接训练机器人，指定任务：
  Use the `scripts/rsl_rl/train.py` script to train the robot directly, specifying the task:

```bash
python3 scripts/rsl_rl/train.py --task=Isaac-Limx-PF-Blind-Flat-v0 --headless
```

- 以下参数可用于自定义训练：
  The following arguments can be used to customize the training:
    * --headless: 以无渲染模式运行仿真 / Run the simulation in headless mode
    * --num_envs: 要运行的并行环境数量 / Number of parallel environments to run
    * --max_iterations: 最大训练迭代次数 / Maximum number of training iterations
    * --save_interval: 保存模型的间隔 / Interval to save the model
    * --seed: 随机数生成器的种子 / Seed for the random number generator

## 运行训练好的模型 / Playing the trained model

- 要运行训练好的模型：
  To play a trained model:

```bash
python3 scripts/rsl_rl/play.py --task=Isaac-Limx-PF-Blind-Flat-Play-v0 --checkpoint_path=path/to/checkpoint
```

- 以下参数可用于自定义运行：
  The following arguments can be used to customize the playing:
    * --num_envs: 要运行的并行环境数量 / Number of parallel environments to run
    * --headless: 以无头模式运行仿真 / Run the simulation in headless mode
    * --checkpoint_path: 要加载的检查点路径 / Path to the checkpoint to load

## 在Mujoco中运行导出模型（仿真到仿真）/ Running exported model in mujoco (sim2sim)

- 运行模型后，策略已经保存。您可以将策略导出到mujoco环境，并参照在github开源的部署工程[tron1-rl-deploy-python](https://github.com/limxdynamics/tron1-rl-deploy-python)在[pointfoot-mujoco-sim](https://github.com/limxdynamics/pointfoot-mujoco-sim)中运行。

  After playing the model, the policy has already been saved. You can export the policy to mujoco environment and run it in mujoco [pointfoot-mujoco-sim]((https://github.com/limxdynamics/pointfoot-mujoco-sim)) by using the [tron1-rl-deploy-python]((https://github.com/limxdynamics/tron1-rl-deploy-python)).

- 按照说明正确安装，并用您训练的`policy.onnx`和`encoder.onnx`替换原始文件。

  Following the instructions to install it properly and replace the origin policy by your trained `policy.onnx` and `encoder.onnx`.

## 在真实机器人上运行导出模型（仿真到现实）/ Running exported model in real robot (sim2real)
<p align="center">
    <img alt="Figure2 of CTS" src="./media/learning_frame.png">
</p>

**学习框架概述 / Overview of the learning framework.**

- 策略使用PPO在异步actor-critic框架内进行训练，动作由历史观察信息编码器和本体感受确定。**灵感来自论文CTS: Concurrent Teacher-Student Reinforcement Learning for Legged Locomotion. ([H. Wang, H. Luo, W. Zhang, and H. Chen (2024)](https://doi.org/10.1109/LRA.2024.3457379))**

  The policies are trained using PPO within an asymmetric actor-critic framework, with actions determined by history observations latent and proprioceptive observation. **Inspired by the paper CTS: Concurrent Teacher-Student Reinforcement Learning for Legged Locomotion. ([H. Wang, H. Luo, W. Zhang, and H. Chen (2024)](https://doi.org/10.1109/LRA.2024.3457379))**

- 实机部署详情见 https://support.limxdynamics.com/docs/tron-1-sdk/rl-training-results-deployment 8.1~8.2章节

  Real deployment details see section https://support.limxdynamics.com/docs/tron-1-sdk/rl-training-results-deployment 8.1 ~ 8.2


## 视频演示 / Video Demonstration

### Isaac Lab中的仿真 / Simulation in Isaac Lab
- **点足盲目平地 / Pointfoot Blind Flat**:

![play_isaaclab](./media/play_isaaclab.gif)
### Mujoco中的仿真 / Simulation in Mujoco
- **点足盲目平地 / Pointfoot Blind Flat**:

![play_mujoco](./media/play_mujoco.gif)

### 真实机器人部署 / Deployment in Real Robot
- **点足盲目平地 / Pointfoot Blind Flat**:

![play_mujoco](./media/rl_real.gif)

## 致谢 / Acknowledgements

本项目使用以下开源库：
This project uses the following open-source libraries:
- [IsaacLabExtensionTemplate](https://github.com/isaac-sim/IsaacLabExtensionTemplate)
- [rsl_rl](https://github.com/leggedrobotics/rsl_rl/tree/master)
- [bipedal_locomotion_isaaclab](https://github.com/Andy-xiong6/bipedal_locomotion_isaaclab)
- [tron1-rl-isaaclab](https://github.com/limxdynamics/tron1-rl-isaaclab)

**贡献者 / Contributors:**
- Hongwei Xiong 
- Bobin Wang
- Wen
- Haoxiang Luo
- Junde Guo

