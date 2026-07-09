<div align="center">

# 🌀 FractalExplorer —— 分形艺术探索器

[![Qt5](https://img.shields.io/badge/Framework-Qt5-41CD52?style=for-the-badge&logo=qt&logoColor=white)](https://www.qt.io)
[![C++11](https://img.shields.io/badge/Language-C%2B%2B11-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)](https://isocpp.org)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/windows)

**📌 基于 Qt5 的 Mandelbrot / Julia 集合可视化交互工具**

👩‍🎓 **作者：** 中国地质大学（北京）**傅依依**
📅 **项目类型：** C++ 课程设计项目

---

</div>

## 🌟 项目简介

> ✨ **分形（Fractal）** 是一类无限复杂、具有自相似结构的几何图形。在复平面上，通过反复迭代一个简单公式，就能生成令人惊叹的图案——无限放大后，每一层都藏着全新的世界。

本项目实现了两个最经典的分形集合：

| 🎯 分形 | 📐 公式 | 💡 特点 |
|---------|---------|--------|
| **Mandelbrot 集合** | `Zₙ₊₁ = Zₙ² + C`，`Z₀ = 0` | 经典的"葫芦形"，边缘无限复杂 |
| **Julia 集合** | `Zₙ₊₁ = Zₙ² + K`，`Z₀ = C` | 根据参数 K 的不同呈现千变万化的形态 |

🧩 用户可以 **🖱️ 实时交互探索** —— 拖拽平移、滚轮缩放、框选放大、切换配色，感受数学之美。

---

## 🚀 功能特性

### ⚡ 核心功能

| 功能 | 说明 |
|------|------|
| 🔄 **Mandelbrot / Julia 双引擎** | 一键切换分形类型 |
| 🎨 **6 种配色方案** | 🌈彩虹 🔥火焰 🌊海洋 ⚪灰度 ✨星空 🌋熔岩 |
| 🖱️ **实时交互** | 鼠标拖拽平移、滚轮缩放、Shift+框选区域放大 |
| 🌈 **平滑着色** | 基于迭代次数的连续颜色映射，避免色带断层 |
| ⚙️ **可调渲染精度** | 迭代次数 16~2000 可调 |
| 🎛️ **Julia 参数实时调节** | 改变 K 值实时观察集合形态变化 |
| 📸 **截图保存** | 导出当前视图为 PNG 图片 |
| 🌙 **暗色主题 UI** | 深色界面，视觉舒适 |

### 🎮 操作说明

| ⌨️ 操作 | ✋ 方式 |
|---------|--------|
| 🖐️ 平移 | 鼠标左键拖拽 |
| 🔍 缩放 | 鼠标滚轮（以光标位置为中心） |
| 📐 框选放大 | 按住 `Shift` + 鼠标左键拖拽框选 |
| 🔄 重置视图 | 菜单 `View → Reset` 或面板按钮 |
| ⚙️ 切换分形/配色/迭代 | 右侧控制面板 |

---

## 🏗️ 技术架构

```
FractalExplorer/
├── 📄 FractalExplorer.pro    # Qt 项目文件
├── 📄 main.cpp               # 主入口：窗口布局、菜单栏、信号连接
├── 📄 fractalengine.h/.cpp   # 核心引擎：分形计算 + 颜色映射
├── 📄 fractaldrawer.h/.cpp   # 画布控件：渲染、鼠标/滚轮/框选交互
├── 📄 controlpanel.h/.cpp    # 控制面板：参数调节、暗色样式
└── 📄 README.md
```

### 📐 类的设计（面向对象三大特性全覆盖）

```
                    QObject
                       |
                 FractalEngine          🧩 (抽象基类)
                 /          \
    MandelbrotEngine      JuliaEngine    🔗 (继承 + 多态：虚函数 computePoint)
                                             |
              FractalDrawer : QWidget     📦 (封装：自定义绘图控件)
              ControlPanel  : QWidget     📦 (封装：控制面板)

              ColorMapper                🛠️ (工具类：迭代次数 → 颜色)
```

### 🔧 关键技术点

| 🏆 技术 | 📍 应用位置 | 📋 对应评分标准 |
|---------|----------|-------------|
| **继承与多态** | `FractalEngine` 抽象基类 → `MandelbrotEngine`/`JuliaEngine` 派生类，虚函数 `computePoint()` | ✅ 面向对象核心 |
| **封装** | 所有数据成员 `private`/`protected`，通过 public 接口访问 | ✅ 代码质量 |
| **STL** | `std::complex` 复数运算，`std::vector` 辅助数据结构 | ✅ STL 应用 |
| **QT5 自定义控件** | `FractalDrawer` 继承 QWidget，重写 `paintEvent`/`mousePressEvent`/`wheelEvent` | ✅ GUI 编程 |
| **QT5 信号与槽** | 控制面板 ↔ 画布 ↔ 引擎之间的解耦通信 | ✅ 事件驱动 |
| **Qt 资源管理** | `QImage` 像素级渲染，`QPainter` 绘制，`QSplitter` 布局 | ✅ 图形编程 |

### 🔄 渲染流程

```
🖱️ 用户交互（拖拽/滚轮/框选）
    ↓
⚙️ 更新引擎参数（center, scale, maxIter）
    ↓
🚀 触发 render()
    ↓
🖥️ 逐像素调用 FractalEngine::computePoint(cx, cy, maxIter)
    ↓                   ↓
📐 Mandelbrot: Z=Z²+C    Julia: Z=Z²+K
    ↓
🎨 ColorMapper::map(iter, maxIter, palette)
    ↓
💾 写入 QImage 像素缓冲区
    ↓
🖼️ paintEvent() → QPainter::drawImage()
```

---

## 🔨 构建与运行

### 📋 环境要求

| 🖥️ 框架 | ⚙️ 编译器 | 📝 语言标准 |
|---------|----------|------------|
| Qt 5.2.1+ | MinGW 4.8+ / MSVC 2013+ | C++11 |

### 📦 构建步骤

```bash
# 1️⃣ 生成 Makefile
qmake FractalExplorer.pro

# 2️⃣ 编译
mingw32-make   # MinGW
# 或
nmake          # MSVC

# 3️⃣ 运行
./release/FractalExplorer.exe
```

💡 或在 **Qt Creator** 中直接打开 `FractalExplorer.pro`，配置编译套件后点击运行。

> ⚠️ **注意：** Qt Creator 需设置默认编码为 UTF-8（工具 → 选项 → 文本编辑器 → 行为 → 默认编码）。

---

## 🗺️ 探索建议

以下是几个值得放大的有趣坐标（在 **Mandelbrot** 模式下）：

| 📍 位置 | 🔬 缩放级别 | 👀 看点 |
|---------|---------|------|
| `(-0.75, 0.1)` | `0.5` | 🐉 Seadragon Valley |
| `(-0.745, 0.113)` | `1e-2` | 🐴 Seahorse Valley |
| `(-1.25, 0.05)` | `0.3` | 🐘 Elephant Valley |
| `(0.28, 0.008)` | `1e-5` | 🔬 微型 Mandelbrot 复制体 |

---

## 📈 后续可扩展方向

- [x] 🎯 **核心分形引擎**（Mandelbrot / Julia）
- [ ] 🔄 **多线程渲染**（QThreadPool 并行计算）
- [ ] 🔥 **更多分形类型**（Burning Ship、Newton 分形）
- [ ] 🎬 **动画模式**（K 值渐变 / 缩放动画）
- [ ] 🖼️ **导出高分辨率图片**
- [ ] 🖱️ **鼠标悬停显示复平面坐标**
- [ ] 📜 **历史记录**（前进/后退导航）

---

## 🙏 致谢

- 📖 参考：《C++程序设计课程设计 2026》实习指导书
- 🧩 Qt 框架：[qt.io](https://www.qt.io)
- 🧮 分形数学：Mandelbrot, B. (1980)

---

<div align="center">

**Made with ❤️ by 傅依依 · 中国地质大学（北京）**

[![GitHub](https://img.shields.io/badge/GitHub-fuyiyi36--droid-181717?style=flat-square&logo=github)](https://github.com/fuyiyi36-droid)

</div>