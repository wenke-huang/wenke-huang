## 黄文珂 · Wenke Huang

哈尔滨工业大学（深圳）· 自动化本科（2028 届）· 具身智能 / 视觉-语言-动作模型（VLA）

我在意的不只是把 VLA 训到 loss 下降，而是它能不能在真机上稳定执行——参数高效微调的秩该给多少、
部署链路会在哪里静默失效、评测指标怎么设计才不至于自欺。

*Undergrad at HITSz. I work on embodied AI, and specifically on whether a VLA policy actually
works on the robot — not only on whether its training loss goes down.*

> 寻找具身智能 / VLA 方向的科研实习：2026.11 起，每周 4 天，6 个月以上。

---

### 当前工作

挑战杯「揭榜挂帅」专项赛 XH-202611《人形机器人具身智能操作技术研究》（发榜：乐聚智能）。
在 Kuavo 5 双臂人形上把 π0.5 适配到四项工业操作（料盘出库 / 零件分拣 / USB 插接 / 快递称重），
负责模型训练与真机部署链路。

### 代表性仓库

**[kuavo-pi05-vla](https://github.com/wenke-huang/kuavo-pi05-vla)** — π0.5 微调与真机部署的工程记录

- 框架默认的 LoRA 配方因目标正则失效，只训到 38 个模块、129 万参数：语言主干一个适配器都没有，
  视觉塔全程冻结。loss 照样下降，曲线上完全看不出来
- 对权重更新 ΔW 做 SVD 实测各模块的秩需求：视觉塔 `k@0.99` 中位 704 / 满秩 1152（只能是全参），
  语言主干 8–11，动作专家 5–6
- 真机动作不连贯的根因是 `meta` + `to_empty` 加载时非持久缓冲区没有被重建，而不是模型容量不足
- 每个数字都能在 `PROVENANCE.md` 追到出处；追不到出处的明确标为不计入

**[torch-buffer-checkpoint-pitfall](https://github.com/wenke-huang/torch-buffer-checkpoint-pitfall)** — 上面那个根因的最小可运行复现

- 不依赖 GPU 的确定性复现：`register_buffer(persistent=False)` 的缓冲区不在 `state_dict` 里，
  checkpoint 流式加载永远写不到它们，加载不报错、推理不抛异常、输出仍然有限
- 附 fail-closed 的修复方案与 7 个回归测试，把一次静默的真机故障变成 CI 里能复现的断言

### 技能

PyTorch · PEFT/LoRA · π0.5 / ACT / Diffusion Policy · 真机部署与评测 · ROS · C/C++ · Python · Linux

### 关于本主页

比赛的数据与权重归发榜单位，训练与部署代码是团队协作产物，均不适合公开。
这里放的是能独立公开、且能自证的部分：机制、测量、负结果，以及可以重新推导的分析。

### 联系

2024312120@stu.hit.edu.cn
