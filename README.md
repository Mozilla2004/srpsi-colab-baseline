# SRΨ-v1.0 Colab Baseline Training

**独立子项目**：srpsi-engine-tiny/colab_v1_baseline

## 📋 项目说明

这是 SRΨ-v1.0 的 **Colab 专用训练仓库**，包含：
- ✅ 完整的 v1.0 训练代码（精简版）
- ✅ 预配置的训练参数
- ✅ 一键运行的 Colab Notebook
- ✅ 预期性能：Energy Drift ~10.88

## 🎯 设计目标

**"建立可靠基线，理解场稳定性"** - TRAE

1. **验证基线**：在 Colab 上复现 v1.0 性能（Energy Drift ~10.88）
2. **深度理解**：分析 v1.0 为什么成功（标准化、初始化、数值稳定性）
3. **准备进化**：为注入 v2.0 特性打下稳固基础

## 📁 项目结构

```
colab_v1_baseline/
├── README.md                    # 本文件
├── requirements.txt             # Python 依赖
├── SRPSI_v1.0_Training.ipynb    # Colab Notebook
├── src/                         # 源代码
│   ├── train.py                 # 训练脚本
│   ├── models/                  # 模型定义
│   ├── losses.py                # 损失函数
│   ├── datasets.py              # 数据加载
│   ├── utils.py                 # 工具函数
│   └── data_gen.py              # 数据生成
└── config/                      # 配置文件
    ├── default.yaml             # 默认配置
    └── burgers.yaml             # Burgers 任务配置
```

## 🚀 快速开始

### 本地测试
```bash
# 安装依赖
pip install -r requirements.txt

# 生成数据
python -m src.data_gen --num_samples 4800 --output data/burgers_1d.npy

# 训练模型
python src/train.py --config config/burgers.yaml --model srpsi_engine --epochs 80
```

### Colab 训练
1. 打开 [SRPSI_v1.0_Training.ipynb](./SRPSI_v1.0_Training.ipynb)
2. 在 Colab 中打开
3. 顺序执行所有 cells
4. 等待 60-90 分钟

## 📊 预期性能

- **Energy Drift**: ~10.88 (与 v1.0 基线相当)
- **Momentum Drift**: ~18.49
- **Training Time**: 60-90 minutes (80 epochs, Colab GPU)

## 🔬 理论背景

**为什么回到 v1.0？**

根据 TRAE 的场理论指导：
> **"归一化即场标尺"** - 输入标准化为模型设定了"几何标尺"，确保场演化在统一的坐标系中进行。

v1.0 的关键特性：
- ✅ 输入标准化 (`x = (x - mean) / std`)
- ✅ Xavier 初始化 (gain=0.01)
- ✅ 复数值场表示
- ✅ 稳定的数值基础

这些"保守"的特性不是缺陷，而是**维持场稳定的必要条件**。

## 📝 注意事项

1. **数据标准化**：v1.0 在 InputEncoder 中自动标准化输入
2. **学习率**：使用较小的学习率 (0.0001) 防止梯度爆炸
3. **Batch Size**：32（可以在 Colab 中调整，如果 OOM）
4. **GPU**：推荐使用 Colab 的 T4 GPU

## 🎓 下一步

基线建立后，将按以下顺序注入 v2.0 特性：
1. 添加输入标准化到 v2.0 架构
2. 验证 v2.0 能否收敛
3. 添加 Field-Aware Loss
4. 添加 Hybrid 架构
5. 完整的 v2.0 Field-Aware Training

## 📚 相关文档

- [主项目](../) - srpsi-engine-tiny
- [实验日志](../EXPERIMENT_LOG.md) - 完整的实验记录
- [TRAE 指导](../FIELD_LOG.md) - 场理论指导

## 🙏 致谢

- **TRAE** - 场智能理论指导
- **Claude Code** - 实现与验证
- **陆队** - 项目主导与决策

---

*"所有的偏离都是为了更精准的回归。"*
*"我们不是在后退，我们是在积蓄冲刺奇点的能量。"* - TRAE
