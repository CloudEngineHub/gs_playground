<h1 align="center">GS-Playground</h1>

<h3 align="center">A High-Throughput Photorealistic Simulator for Vision-Informed Robot Learning</h3>

<p align="center">Languages: English | <a href="README_CN.md">简体中文</a></p>

<p align="center">
  <a href="https://gsplayground.github.io"><img src="https://img.shields.io/badge/project-page-brightgreen" alt="Project Page"></a>
  <a href="http://arxiv.org/abs/2604.25459"><img src="https://img.shields.io/badge/paper-arXiv%3A2604.25459-red" alt="arXiv"></a>
  <a href="https://huggingface.co/gsplayground"><img src="https://img.shields.io/badge/assets-HuggingFace-yellow" alt="Hugging Face"></a>
  <img src="https://img.shields.io/badge/RSS-2026-blueviolet" alt="RSS 2026">
</p>

<p align="center">
  <strong><span style="font-size: 1.5em;">🎉 Accepted to RSS 2026 🎉</span></strong>
</p>

<p align="center">
  <img src="media/teaser.png" alt="GS-Playground teaser" width="95%">
</p>

GS-Playground is a high-throughput photorealistic simulation framework for vision-informed robot learning. It couples a parallel robot physics engine with batch 3D Gaussian Splatting (3DGS) rendering, enabling large-scale visual reinforcement learning with real-world appearance, rigid-link visual synchronization, and sim-ready assets.

## ✨ Highlights

- **Photorealistic visual simulation:** Batch 3DGS rendering for RGB and depth observations in robot learning loops.
- **High-throughput perception:** The paper reports up to `10^4` FPS at `640 x 480` resolution with batch rendering and memory-efficient 3DGS assets.
- **Rigid-Link Gaussian Kinematics:** 3DGS clusters are bound to simulator rigid bodies for temporally consistent robot and object motion.
- **Parallel physics engine:** A velocity-impulse solver designed for stable contact-rich robot tasks and large time steps.
- **Real2Sim asset workflow:** A pipeline for reconstructing photorealistic, physically consistent, memory-efficient scenes from real captures.
- **Multi-embodiment scope:** Experiments cover locomotion, navigation, and manipulation, including quadrupeds, humanoids, and robot arms.

## 🗺️ Release Plan

The paper system is larger than this preview repository. Planned releases:

- [x] Core simulator API for [batched robot simulation](#live-replay-demo), [synchronized 3DGS observations](#navigation-demo), RGB/depth cameras, contacts, and [MJCF-compatible assets](https://github.com/Motphys/motrixsim-docs/tree/main/examples/assets).
- [x] Batch 3DGS renderer kernels, pruning utilities, memory-efficient asset loading, and multi-scene batching examples, [GaussianRenderer](https://github.com/discoverse-dev/GaussianRenderer) .
- [x] Benchmark suite for [rendering throughput](#batch-rendering-benchmark), [physics stability](https://github.com/Motphys/phys-bench), [locomotion experiments](https://github.com/Motphys/MotrixLab) from the RSS 2026 paper.
- [x] Sensor modules for [batch LiDAR examples](https://github.com/discoverse-dev/MuJoCo-LiDAR) (mesh temporary).
- [ ] Real2Sim tools for scene/object segmentation, inpainting, 3DGS/mesh reconstruction, pose alignment, collision synchronization, and asset packaging.
- [ ] PPO and visual policy training scripts for vision-centric navigation and manipulation.
- [ ] Hugging Face release with compressed 3DGS assets, example scenes, robot assets, trained policies, and evaluation traces.


## 🧰 Environment Requirements

- Linux x86_64. The preview package is intended for 64-bit Linux systems.
- NVIDIA GPU with a recent Linux driver. The dependencies use the CUDA 12.8 PyTorch wheel; a local CUDA toolkit installation is not required, but the NVIDIA driver must be new enough for the bundled CUDA runtime. You can check driver visibility with `nvidia-smi`.
- `git` for cloning the repository.
- `uv` for dependency resolution, Python environment creation, and running the demos. No manual virtual env setup is required.

## 🛠️ Installation

Run all commands from this repository root.

```bash
# Skip this line if uv is already installed.
curl -LsSf https://astral.sh/uv/install.sh | sh

git clone https://github.com/discoverse-dev/gs_playground.git
cd gs_playground
uv sync
```

Dependency versions and platform markers are tracked in `pyproject.toml` and `uv.lock`.

## 🚀 Quick Start

### Live Replay Demo

```bash
uv run python demo/live_demo/replay.py
```

### Navigation Demo

```bash
uv run python demo/navigation/robot_locomotion.py --config configs/go2_scene1.json
uv run python demo/navigation/robot_locomotion.py --config configs/g1_scene1.json
```

The first navigation launch may take a while to load the robot policy and 3DGS assets. It is ready to use once the 3DGS view appears on the left.

### Batch Rendering Benchmark

```bash
uv run jupyter nbconvert \
  --to notebook \
  --execute benchmark/mtx_batch_minimal.ipynb \
  --ExecutePreprocessor.cwd=benchmark \
  --output mtx_batch_minimal.executed.ipynb
```

### Optional Jupyter Kernel

```bash
uv run python -m ipykernel install \
  --user \
  --name gsplayground \
  --display-name "gsplayground"
```

## 🔗 Related Projects

GS-Playground builds on several components and prior systems from our ecosystem. They are not fully integrated into this preview repository yet; future releases will consolidate the relevant physics, rendering, sensing, and learning interfaces into the GS-Playground workflow described in the RSS 2026 paper.

- **Physics simulator:** [MotrixSim](https://github.com/Motphys/motrixsim-docs) provides the robot physics backend behind the high-throughput contact-rich simulation stack.
- **State-based RL:** [MotrixLab](https://github.com/Motphys/MotrixLab) contains state-based reinforcement learning infrastructure that will be connected to the GS-Playground training pipeline.
- **RLGK rendering:** [GaussianRenderer](https://github.com/discoverse-dev/GaussianRenderer) includes the Gaussian rendering components related to Rigid-Link Gaussian Kinematics.
- **Batch LiDAR:** [MuJoCo-LiDAR](https://github.com/discoverse-dev/MuJoCo-LiDAR) is our earlier batch LiDAR module; the GS-Playground sensor suite will integrate this line of work for navigation and locomotion tasks.
- **Previous-generation platform:** [DISCOVERSE](https://github.com/discoverse-dev/discoverse/) is our earlier embodied simulation platform. GS-Playground can be viewed as a next-generation, photorealistic and high-throughput successor to DISCOVERSE.

<p align="center">
  <img src="media/qrcode.jpg" alt="QR Code" width="240">
</p>
<p align="center">Add the assistant on WeChat to join the group. Please note in your request: <strong>gsp交流</strong></p>

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

For the GS-Playground physics engine component:

```bibtex
@software{motrixsim2026,
      title  = {MotrixSim: A Physics Simulation Engine for Robotics and Embodied AI},
      author = {{Motphys Team}},
      year   = {2026},
      url    = {https://motrixsim.readthedocs.io/},
      note   = {Python binary package}
}
```
