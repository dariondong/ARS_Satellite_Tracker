![app_icon.jpg](https://raw.gitcode.com/user-images/assets/10539895/079bdd1c-88af-4ddd-b168-75a00a990d5c/app_icon.jpg 'app_icon.jpg')

# ARS 卫星追踪系统

![CODE](https://img.shields.io/badge/CODE-Python%20C%23-brightgreen?style=flat-square&labelColor=555)

基于 Python + Flask + C# + Three.js 的实时卫星追踪桌面应用，支持 3D 地球可视化、屏保、系统托盘、安装包部署。

## Stargazers over time

![Stargazers over time](https://gitcode.com/DarionDong/ARS_Satellite_Tracker/starcharts.svg?variant=adaptive)

## 功能特性

- **3D 地球可视化** — Three.js 渲染地球、卫星轨道线、覆盖区域，支持鼠标滚轮缩放和拖拽旋转
- **实时卫星追踪** — 基于 SGP4 轨道模型，实时计算卫星位置、仰角、速度、距离等数据
- **系统托盘** — 最小化到托盘运行，Alt+L 快速切换禅模式，启动自动打开窗口
- **禅模式（屏保）** — 系统空闲时自动全屏，自动追踪最佳过境卫星，显示实时数据和下个过境预测
- **双击桌面图标激活窗口** — 已有实例运行时自动拉到前台，不重复启动
- **地面站管理** — 支持添加/删除/切换地面站，选择自动记忆和恢复
- **右侧详情面板** — 卫星详细信息、实时数据图表（仰角/距离/方位角/速度曲线）
- **Windows 安装包** — Inno Setup 打包，支持桌面快捷方式、开机自启、.scr 屏保安装

## 技术栈

| 层级 | 技术 |
|------|------|
| 后端 | Python 3, Flask, SGP4 |
| 前端 | Three.js, 原生 JavaScript, HTML5/CSS3 |
| 桌面 | C# (.NET 8), WebView2, WinForms |
| 打包 | Inno Setup, dotnet publish (self-contained) |
| 数据 | TLE 轨道数据, Celestrak API |

## 快速开始

### 开发模式

```bash
pip install -r requirements.txt
python app.py
```

浏览器访问 `http://127.0.0.1:5000`。

### 构建桌面应用

```bash
cd ARS_Screensaver
build.bat
```

输出：
- `bin\Release\net8.0-windows\` — 开发版（framework-dependent）
- `bin\Release\net8.0-windows\publish\` — 自包含单文件版
- `installer\ARS_Setup_v1.0.0.exe` — 安装包

### 安装

运行 `installer\ARS_Setup_v1.0.0.exe`，安装到 `%LOCALAPPDATA%\Programs\ARS\`。

安装后：
- 双击桌面图标启动应用
- .scr 屏保文件自动注册到系统
- 支持开机自启（可选）

## 禅模式

- 按 `Alt+L` 或托盘菜单进入禅模式
- 进入后延迟 2 秒，自动追踪最佳过境卫星
- 自动切换卫星（最低追踪 20 秒）
- 左上角显示当前时间、日期、星期
- 右上角显示地面站、卫星实时数据、下个过境预测
- 按 `Esc` 退出禅模式

## 项目结构

```
ars_APP/
├── app.py                       # Flask 后端
├── desktop_app.py               # Python 桌面入口（旧版）
├── requirements.txt             # Python 依赖
├── ground_stations.json         # 地面站数据
├── active_station.json          # 当前选择的地面站
├── templates/
│   └── index.html               # 前端主页面
├── static/
│   └── textures/                # 地球纹理贴图
├── ARS_Screensaver/             # C# 桌面应用
│   ├── Program.cs               # 入口，单实例检测
│   ├── TrayApp.cs               # 托盘管理、Flask 生命周期
│   ├── BrowserForm.cs           # 标准窗口（WebView2）
│   ├── ScreensaverForm.cs       # 禅模式全屏窗口
│   ├── FlaskManager.cs          # Python Flask 进程管理
│   ├── NativeMethods.cs         # Win32 API 封装
│   ├── AppHelper.cs             # 注册表路径查找
│   ├── ARS_Screensaver.csproj   # .NET 项目配置
│   ├── build.bat                # 完整构建脚本
│   ├── bundle_python.bat        # Python 运行时打包
│   ├── ARS_Installer.iss        # Inno Setup 安装脚本
│   ├── convert_icon.py          # JPG 转 ICO 脚本
│   └── app_icon.jpg             # 应用图标
└── logs/                        # 运行日志
```

## 依赖

**Python:**
```
Flask==3.0.0
sgp4==2.23
requests==2.31.0
```

**.NET:**
- .NET 8.0 SDK
- Microsoft.Web.WebView2 1.0.2903.40

**构建工具:**
- Inno Setup 6
- Python 3.10+ (embedded)