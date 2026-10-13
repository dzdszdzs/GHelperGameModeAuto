[Uploading README.md…]()
# G-Helper 游戏模式自动切换器 (GHelperGameModeAuto)

<p align="center">
  <img src="app.ico" alt="Logo" width="96" height="96" />
</p>

<p align="center">
  <strong>专为华硕 (ROG / TUF) 笔记本与 G-Helper 打造的超轻量、超低内存、绿色便携游戏模式自动切换工具</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet&logoColor=white" alt=".NET 8.0" />
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011%20(x64)-0078D6?logo=windows&logoColor=white" alt="Windows 10/11" />
  <img src="https://img.shields.io/badge/Memory-2MB~5MB%20RAM-success" alt="Memory" />
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License" />
  <img src="https://img.shields.io/badge/Build-Single--File%20Portable-teal" alt="Portable" />
</p>

---

## 📖 项目简介 (Introduction)

[**G-Helper**](https://github.com/seerge/g-helper) 是一款广受好评的华硕轻量级控制软件，完全替代臃肿的 Armoury Crate（奥创中心）。然而，G-Helper 本身尚未内置**根据运行游戏自动切换性能模式**的功能。

很多玩家在进行 3A 游戏或电竞网游时需要开启 **「增强 (Turbo)」** 或 **「自定义高性能功耗曲线」** 以获得最强帧率；而在日常办公、看视频、浏览网页时又希望保持 **「平衡 (Balanced)」** 或 **「静音 (Silent)」** 以降低风扇噪音和功耗。频繁手动切出游戏按快捷键十分繁琐，且在退出游戏后极易遗忘切换回日常模式。

**GHelperGameModeAuto** 专为此痛点诞生：
* 🎮 **游戏启动**：毫秒级自动切换至指定的性能模式（如 增强 Turbo / 自定义 1 / 自定义 2）。
* 🍃 **游戏退出**：游戏关闭后自动恢复到启动前的基准模式，或者固定切回平衡/静音模式。
* ⚡ **零干扰体验**：全程静默后台托管，开机自启、随心使用，彻底告别手动切换烦恼！

---

## ✨ 核心特性 (Features)

### 1. 🎯 游戏进程自动监听与智能切换
* **多策略退出恢复**：
  * **恢复启动前模式（推荐）**：游戏启动前是静音，退出自动恢复静音；启动前是平衡，退出自动恢复平衡。
  * **固定退出模式**：无论启动前是什么状态，退出后固定切换至指定模式（如平衡或静音）。
* **多游戏并发防抖跟踪**：同时运行多款游戏时（如排队等待另一款游戏），智能持续维持游戏模式，直到所有配置的游戏全部退出后才触发退出恢复，杜绝中途误切。

### 2. 🪶 极致后台性能与超低内存优化 (~3MB RAM)
* **零托管堆分配内核快照扫描**：摒弃高分配开销的 `Process.GetProcesses()`，采用 Windows 底层 `CreateToolhelp32Snapshot` 快照遍历，不为每个系统进程创建托管对象，消除了高频扫描引发的 GC 垃圾回收压力与内存抖动。
* **自适应动态轮询**：无游戏运行的待机状态下自动降低扫描频率（空闲省电），检测到目标游戏启动后自适应提升至活跃响应频率，兼顾秒级触发与极低 CPU 占用。
* **物理工作集深度修剪**：窗口最小化至系统托盘时，协同 Windows 内存管理器将未使用的 UI 与 DirectX 纹理分页释放，后台待机物理内存降至 **2MB ~ 5MB** 级别。

### 3. ⌨️ 成熟可靠的虚拟按键方案 (`keybd_event`)
* **完全免物理键冲突**：默认使用 Windows 系统级虚拟功能键 `F16 ~ F20`（搭配 `Ctrl + Shift + Alt`）。Windows 原生支持扩展功能键 F13~F24，键盘上没有这些物理键也不会与任何常规软件或游戏按键产生冲突。
* **支持快捷键自由定制**：支持全局热键实时录制，也提供 F13~F24 虚拟键直选；界面内置「⚡ 测试发送」按钮，可一键验证 G-Helper 联动效果。

### 4. 🔍 多平台游戏库一键扫描与管理
* **一键自动扫描**：智能检索本机已安装的 **Steam**（支持多盘符游戏库解析）、**Epic Games** 以及 **Xbox / Microsoft Store** 游戏。
* **批量导入与自由定制**：扫描结果可视化清单展示，用户可自由多选、一键设置默认启动模式和退出策略并批量导入；支持手动选择任意游戏 `.exe` 添加。

### 5. 🎨 沉浸式深色界面与 Windows 11 原生风格
* 采用 Windows 原生 DWM 沉浸式深色模式（Dark Caption Titlebar），标题栏与深色界面浑然一体，消除传统 WPF 应用白边刺眼的割裂感。
* 内置完整的高清多尺寸矢量图标（16x16 ~ 256x256），任务栏、标题栏与系统托盘视觉一致。

### 6. 📦 绝对绿色便携 (Portable & Clean)
* **单文件即开即用**：发布为独立单文件可执行程序，无需复杂安装器。
* **本地自包含配置**：配置文件和日志仅存放在程序同级目录的 `config/` 和 `logs/` 中，**绝不污染** `%AppData%`、`%LocalAppData%` 或系统注册表。
* **绿色自启动**：开机自启动使用标准 Startup 快捷方式，关闭自启时一键彻底清理。

---

## 🛠️ 与 G-Helper 配合配置指南 (Setup Guide)

本程序通过模拟系统全局快捷键与 G-Helper 进行联动。只需在 G-Helper 中简单绑定一次快捷键即可完成对接：

### 第一步：在 G-Helper 中绑定模式快捷键
1. 打开 **G-Helper** 主界面。
2. 点击界面底部的 **「Extra」(附加设置)** 按钮。
3. 找到 **按键绑定 (Keybindings)** 区域，为各运行模式依次设置对应的快捷键：
   * **静音 (Silent)**：`Ctrl + Shift + Alt + F16`
   * **平衡 (Balanced)**：`Ctrl + Shift + Alt + F17`
   * **增强 (Turbo)**：`Ctrl + Shift + Alt + F18`
   * **自定义 1 (Custom 1)**：`Ctrl + Shift + Alt + F19`
   * **自定义 2 (Custom 2)**：`Ctrl + Shift + Alt + F20`
4. 点击关闭保存 Extra 设置。

### 第二步：验证快捷键对接
1. 启动 **GHelperGameModeAuto**。
2. 切换至界面的 **「⌨ 快捷键设置」** 标签页。
3. 点击任意模式旁边的 **「⚡ 测试发送」** 按钮：
   * 如果观察到 G-Helper 界面上的模式随之同步改变，说明配置完全成功！

### 第三步：添加游戏
1. 点击 **「🎮 游戏管理」** 标签页。
2. 点击 **「🔍 扫描游戏库」** 快速勾选并导入本机游戏，或点击 **「➕ 手动添加游戏」** 选择游戏的 `.exe` 文件。
3. 设定该游戏的启动模式（如 Turbo）及退出策略即可！

---

## 📂 目录结构说明

```text
GHelperGameModeAuto/
├── GHelperGameModeAuto.exe       # 核心主程序 (单文件绿色版)
├── config/
│   └── settings.json             # 游戏清单、热键及用户偏好设置 (自动生成)
└── logs/
    └── GHelperGameModeAuto.log    # 滚动运行日志 (最大5MB自动滚存)
```

---

## 💻 系统要求 (Requirements)

* **操作系统**：Windows 10 (1809 及以上) / Windows 11 (64-bit)
* **运行环境**：预装 [.NET 8.0 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/8.0)（单文件版已具备高度便携性）
* **依赖硬件/软件**：支持并已安装 [G-Helper](https://github.com/seerge/g-helper) 的华硕笔记本电脑

---

## 🔨 从源码编译 (Build from Source)

如果您希望自行从源代码构建可执行文件：

1. **克隆仓库**：
   ```bash
   git clone https://github.com/<your-username>/GHelperGameModeAuto.git
   cd GHelperGameModeAuto
   ```

2. **还原并编译调试版**：
   ```bash
   dotnet build -c Release
   ```

3. **发布单文件独立运行版**：
   ```bash
   dotnet publish -c Release -r win-x64 --self-contained false /p:PublishSingleFile=true -o "./publish"
   ```
   发布成功后，可执行文件将输出至 `./publish/GHelperGameModeAuto.exe`。

---

## 📄 开源许可证 (License)

本项目基于 [MIT License](LICENSE) 开源，欢迎自由提交 Issue、Pull Request 与 Star 支持！

---

<p align="center">
  如果这个工具改善了您的华硕笔记本使用体验，欢迎在 GitHub 上点亮一颗 ⭐ Star！
</p>
