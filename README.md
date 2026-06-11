# CMP Kinematics Simulation System (CMP 全栈量产运动学分析系统)
https://xumingzhao0401-blip.github.io/CMP-Kinematics-Simulation-System/

这是一个基于 HTML5 Canvas 和原生 JavaScript 开发的轻量级、全栈式化学机械抛光 (CMP) 运动学仿真系统。本工具为半导体制造过程中的平坦化工艺提供直观的理论验证与数据分析。系统无需后端依赖，可直接在浏览器中进行高精度的动态物理场渲染与数据积分。

## ✨ 核心功能模块

本系统将复杂的 CMP 运动学拆解为四个量产核心分析模块：

1. **Wafer NUV & 剖面预测 (Non-Uniformity Velocity)**
   * 模拟晶圆 (Wafer) 与抛光垫 (Pad) 在不同转速比下的相对线速度积分。
   * 支持等速扫掠 (Linear) 与正弦扫掠 (Sine) 模式。
   * 实时生成 2D 去除率热力图以及 1D 径向切削剖面，计算时间平均非均匀度 (NU)。

2. **Defect 划痕图谱溯源**
   * 用于追踪抛光垫上特定半径处存在的异物 (Particle/Defect) 在抛光过程中划过 Wafer 表面的相对轨迹。
   * 帮助工程师在缺陷检测后，通过形貌特征反向定位机台抛光垫的污染位置。

3. **Pad 长周期修整 (Disk Conditioning)**
   * 仿真修整盘 (Disk) 对抛光垫长达数十分钟至数小时的长周期磨损作用。
   * 支持修整盘扫掠边界与运动模式调整（避免深坑或产生环沟），并输出高精度 Pad 径向损耗剖面。

4. **EPD 终点激光采样分析 (Endpoint Detection)**
   * 模拟光学终点检测传感器在抛光盘下方透过 Laser Window 扫描晶圆表面的过程。
   * 生成 100Hz 采样率下激光在 Wafer 表面的真实物理落点轨迹（第一视角，缺口朝上），以及激光打点半径随时间的连续一维波形。

## 🚀 部署与使用

本项目是纯前端的单页面应用 (SPA)，可以直接利用 GitHub Pages 进行免费托管，非常适合作为在线科研或工程辅助工具。

1. 克隆本仓库到本地。
2. 将核心的 HTML 文件严格命名为 `index.html`（注意避免保存时被系统误命名为 `index.html.html`，否则 GitHub Pages 将无法正确解析默认入口）。
3. 将代码推送到主分支或 `gh-pages` 分支。
4. 在仓库的 `Settings -> Pages` 中开启 GitHub Pages 即可直接通过链接访问。

## 📐 底层运动学原理

系统的抛光去除量预测基于经典的 Preston 方程延伸计算。其核心在于求解晶圆表面任意点在时间尺度下的相对速度积分。

设抛光盘的角速度为 $\omega_p$，晶圆的角速度为 $\omega_c$，晶圆中心相对于抛光盘中心的瞬时偏心距为 $e(t)$。

在考虑晶圆自转角度 $\theta_c = -\omega_c t$ 的局部坐标系转换后，局部点坐标 $(x_t, y_t)$ 定义为：
$$x_t = x \cos(\theta_c) - y \sin(\theta_c)$$
$$y_t = x \sin(\theta_c) + y \cos(\theta_c)$$

该点在全局相对运动下的速度分量为：
$$v_x = -(\omega_p - \omega_c) y_t$$
$$v_y = \omega_p e(t) + (\omega_p - \omega_c) x_t$$

相对线速度模长 $v$ 的计算公式为：
$$v = \sqrt{v_x^2 + v_y^2}$$

在仿真时间段 $T$ 内，系统将上述速度矩阵进行离散化累加积分，从而映射出二维热力图并拟合一维的切削剖面。

## 🛠 技术栈
* **HTML5 Canvas**：用于高性能的动态像素级渲染与积分地图绘制。
* **Vanilla JavaScript**：纯原生 JS 实现动画循环 (`requestAnimationFrame`) 与底层矩阵物理计算。
* **CSS3**：响应式布局与现代化 UI 组件控制台。

## 📄 许可证
[MIT License](LICENSE)
