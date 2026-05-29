<h1 align="center">GS-Playground</h1>

<h3 align="center">A High-Throughput Photorealistic Simulator for Vision-Informed Robot Learning</h3>

<p align="center">语言：<a href="README.md">English</a> | 简体中文</p>

<p align="center">
  <a href="https://gsplayground.github.io"><img src="https://img.shields.io/badge/project-page-brightgreen" alt="Project Page"></a>
  <a href="http://arxiv.org/abs/2604.25459"><img src="https://img.shields.io/badge/paper-arXiv%3A2604.25459-red" alt="arXiv"></a>
  <a href="https://huggingface.co/gsplayground"><img src="https://img.shields.io/badge/assets-HuggingFace-yellow" alt="Hugging Face"></a>
  <img src="https://img.shields.io/badge/RSS-2026-blueviolet" alt="RSS 2026">
</p>

<p align="center">
  <strong><span style="font-size: 1.5em;">🎉 已被 RSS 2026 接收 🎉</span></strong>
</p>

<p align="center">
  <img src="media/teaser.png" alt="GS-Playground teaser" width="95%">
</p>

GS-Playground 是面向视觉机器人学习的高吞吐、高保真仿真框架。系统将并行机器人物理引擎与批量 3D Gaussian Splatting (3DGS) 渲染结合，为视觉强化学习、导航、操作和运动控制提供接近真实外观的观测、刚体同步的视觉资产，以及可用于训练的 sim-ready 场景。

## ✨ 亮点

- **高保真视觉仿真：** 使用批量 3DGS 渲染为机器人学习环路提供 RGB 和 depth 观测。
- **高吞吐感知：** 论文中报告系统在 `640 x 480` 分辨率下通过批量渲染和内存高效 3DGS 资产达到最高 `10^4` FPS。
- **Rigid-Link Gaussian Kinematics：** 将 3DGS 点云簇绑定到仿真刚体，保证机器人和物体运动时的时间一致性。
- **并行物理引擎：** 基于 velocity-impulse formulation 的稳定接触求解器，支持 contact-rich 任务和大步长仿真。
- **Real2Sim 资产流程：** 从真实采集生成照片级、物理一致、内存高效的仿真资产。
- **多构型机器人覆盖：** 论文实验覆盖 locomotion、navigation 和 manipulation，包括四足、人形和机械臂。

## 🗺️ Release 计划

论文系统比当前预览仓库更完整，后续计划发布：

- [x] 核心 simulator API：支持 [batched robot simulation](#live-replay-demo)、[同步 3DGS 观测](#navigation-demo)、RGB/depth camera、contact，以及 [MJCF 兼容资产](https://github.com/Motphys/motrixsim-docs/tree/main/examples/assets)。
- [x] Batch 3DGS renderer kernel、剪枝工具、内存高效资产加载和多场景 batch 示例，[GaussianRenderer](https://github.com/discoverse-dev/GaussianRenderer)。
- [x] RSS 2026 论文中的 [rendering throughput](#batch-rendering-benchmark)、[physics stability](https://github.com/Motphys/phys-bench)、[locomotion experiments](https://github.com/Motphys/MotrixLab) benchmark suite。
- [x] [batch LiDAR 示例](https://github.com/discoverse-dev/MuJoCo-LiDAR)传感器模块（mesh temporary）。
- [ ] Real2Sim 工具：场景/物体分割、inpainting、3DGS/mesh 重建、位姿对齐、碰撞同步和资产打包。
- [ ] 面向视觉中心导航和操作的 PPO 与视觉策略训练脚本。
- [ ] Hugging Face 发布：压缩 3DGS 资产、示例场景、机器人资产、训练策略和评测轨迹。

## 🧰 环境需求

- Linux x86_64。当前预览版本面向 64 位 Linux 系统。
- NVIDIA GPU 和较新的 Linux 驱动。依赖使用 CUDA 12.8 PyTorch wheel；不需要单独安装本地 CUDA toolkit，但 NVIDIA 驱动需要足够新，以兼容 wheel 中自带的 CUDA runtime。可通过 `nvidia-smi` 检查驱动和 GPU 是否可见。
- `git`，用于克隆仓库。
- `uv`，用于解析依赖、创建 Python 环境并运行 demo。不需要手动创建 virtual env。

## 🛠️ 安装

以下命令均在仓库根目录执行。

```bash
# 如果已经安装过 uv，跳过这一行。
curl -LsSf https://astral.sh/uv/install.sh | sh

git clone https://github.com/discoverse-dev/gs_playground.git
cd gs_playground
uv sync
```

依赖版本和平台标记由 `pyproject.toml` 与 `uv.lock` 维护。

## 🚀 快速开始

### Live Replay Demo

```bash
uv run python demo/live_demo/replay.py
```

### Navigation Demo

```bash
uv run python demo/navigation/robot_locomotion.py --config configs/go2_scene1.json
uv run python demo/navigation/robot_locomotion.py --config configs/g1_scene1.json
```

首次启动 navigation 可能需要较长时间加载机器人策略和 3DGS 资产。等左侧 3DGS 渲染窗口出现后即可正常使用。

### Batch Rendering Benchmark

```bash
uv run jupyter nbconvert \
  --to notebook \
  --execute benchmark/mtx_batch_minimal.ipynb \
  --ExecutePreprocessor.cwd=benchmark \
  --output mtx_batch_minimal.executed.ipynb
```

### 可选 Jupyter Kernel

```bash
uv run python -m ipykernel install \
  --user \
  --name gsplayground \
  --display-name "gsplayground"
```

## 🔗 相关项目

GS-Playground 建立在我们生态中的多个组件和前序系统之上。它们目前尚未完整整合进这个预览仓库；后续 release 会将 RSS 2026 论文中涉及的物理、渲染、传感和学习接口逐步统一到 GS-Playground 工作流中。

- **物理仿真器：** [MotrixSim](https://github.com/Motphys/motrixsim-docs) 是高吞吐、接触丰富机器人仿真栈背后的物理后端。
- **State-based RL：** [MotrixLab](https://github.com/Motphys/MotrixLab) 包含 state-based reinforcement learning 基础设施，后续会与 GS-Playground 训练流水线连接。
- **RLGK 渲染：** [GaussianRenderer](https://github.com/discoverse-dev/GaussianRenderer) 包含与 Rigid-Link Gaussian Kinematics 相关的 Gaussian rendering 组件。
- **Batch LiDAR：** [MuJoCo-LiDAR](https://github.com/discoverse-dev/MuJoCo-LiDAR) 是我们此前的 batch LiDAR 模块；GS-Playground 的传感器套件会沿着这条工作整合到 navigation 和 locomotion 任务中。
- **上一代平台：** [DISCOVERSE](https://github.com/discoverse-dev/discoverse/) 是我们此前的具身仿真平台。GS-Playground 可以看作 DISCOVERSE 的下一代高保真、高吞吐版本。

<p align="center">
  <img src="media/qrcode.jpg" alt="二维码" width="240">
</p>
<p align="center">添加小助手的微信进群，请备注：<strong>gsp交流</strong></p>

## 📚 Citation

```bibtex
@article{jia2025discoverse,
      title={DISCOVERSE: Efficient Robot Simulation in Complex High-Fidelity Environments},
      author={Yufei Jia and Guangyu Wang and Yuhang Dong and Junzhe Wu and Yupei Zeng and Haonan Lin and Zifan Wang and Haizhou Ge and Weibin Gu and Chuxuan Li and Ziming Wang and Yunjie Cheng and Wei Sui and Ruqi Huang and Guyue Zhou},
      journal={arXiv preprint arXiv:2507.21981},
      year={2025},
      url={https://arxiv.org/abs/2507.21981}
}

@article{jia2026gsplayground,
      title={GS-Playground: A High-Throughput Photorealistic Simulator for Vision-Informed Robot Learning},
      author={Yufei Jia and Heng Zhang and Ziheng Zhang and Junzhe Wu and Mingrui Yu and Zifan Wang and Dixuan Jiang and Zheng Li and Chenyu Cao and Zhuoyuan Yu and Xun Yang and Haizhou Ge and Yuchi Zhang and Jiayuan Zhang and Zhenbiao Huang and Tianle Liu and Shenyu Chen and Jiacheng Wang and Bin Xie and Xuran Yao and Xiwa Deng and Guangyu Wang and Jinzhi Zhang and Lei Hao and Zhixing Chen and Yuxiang Chen and Anqi Wang and Hongyun Tian and Yiyi Yan and Zhanxiang Cao and Yizhou Jiang and Hanyang Shao and Yue Li and Lu Shi and Bokui Chen and Wei Sui and Hanqing Cui and Yusen Qin and Ruqi Huang and Lei Han and Tiancai Wang and Guyue Zhou},
      journal={arXiv preprint arXiv:2604.25459},
      year={2026},
      url={https://arxiv.org/abs/2604.25459}
}
```

GS-Playground 的物理引擎组件引用如下：

```bibtex
@software{motrixsim2026,
      title  = {MotrixSim: A Physics Simulation Engine for Robotics and Embodied AI},
      author = {{Motphys Team}},
      year   = {2026},
      url    = {https://motrixsim.readthedocs.io/},
      note   = {Python binary package}
}
```
