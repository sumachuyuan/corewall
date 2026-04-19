<div align="center">
  <h1>Corewall</h1>
  <p><b>将网页、视频和交互式 URL 转化为精美的 macOS 动态壁纸</b></p>
  <br>
</div>

<p align="center">
  <a href="https://apps.apple.com/app/id6762517792">
    <img src="https://img.shields.io/badge/App_Store-下载-blue?logo=apple&logoColor=white" alt="App Store">
  </a>
  <a href="https://testflight.apple.com/join/8hBCbycu">
    <img src="https://img.shields.io/badge/TestFlight-公测-beta?logo=apple" alt="TestFlight">
  </a>
  <img src="https://img.shields.io/badge/platform-macOS-black?logo=apple" alt="macOS">
  <img src="https://img.shields.io/badge/Swift-5.9-orange?logo=swift" alt="Swift">
</p>

---

## 功能亮点

- **网页即壁纸** — 支持 WebGL、Canvas 和 3D 交互式网页，将任意 URL 变成实时动态桌面
- **4K 视频壁纸** — 基于 AVFoundation 优化的渲染引擎，极低 CPU 占用即可流畅播放 4K 视频
- **在线画廊** — 精选壁纸库，一键下载应用到桌面
- **Lucky Drop** — 随机抽取惊喜壁纸，每次都有新发现
- **电池保护** — 检测电池电量，低电量自动暂停壁纸以节省续航
- **热量管理** — 三级引擎生命周期（active / throttled / absoluteIdle），根据系统温度自动调节
- **多显示器** — 每个屏幕独立壁纸，支持 Space 切换
- **菜单栏控制** — 一键切换、暂停、恢复，无需打开主窗口

## 系统要求

- macOS 14.0 (Sonoma) 或更高版本
- Apple Silicon / Intel 均支持

## 安装

### App Store（推荐）

前往 [App Store](https://apps.apple.com/app/id6762517792) 下载正式版。

### TestFlight

加入 [TestFlight 公测](https://testflight.apple.com/join/8hBCbycu) 获取最新测试版本。

## 从源码构建

### 1. 克隆仓库

```bash
git clone https://github.com/sumachuyuan/corewall.git
cd corewall
```

### 2. 打开 Xcode 项目

```bash
open Corewall.xcodeproj
```

### 3. 配置代码签名

在 Xcode 中选择 `Corewall` target → **Signing & Capabilities** → 选择你的 Developer Team。

### 4. 构建运行

按 **⌘R** 即可运行。Corewall 以菜单栏应用运行 — 在状态栏中查找图标。

或使用命令行：

```bash
xcodebuild -scheme Corewall -configuration Debug build
```

## 项目结构

```
Corewall/
├── Corewall/
│   ├── AppMain.swift              # App 入口
│   ├── AppState.swift             # 全局状态管理
│   ├── DesktopWindow.swift        # 桌面窗口（双层渲染）
│   ├── CorewallEngineView.swift   # 引擎视图（视频 + 网页）
│   ├── LocalAssetImporter.swift   # 本地视频导入
│   ├── WallpaperAsset.swift       # 壁纸数据模型 (SwiftData)
│   ├── StoreKitService.swift      # StoreKit 2 购买服务
│   ├── EntitlementManager.swift   # 权益管理
│   ├── Gallery/                   # 在线画廊模块
│   ├── SSWebView.swift            # 沙盒 WebView
│   ├── Menus.swift                # 菜单栏菜单
│   └── SettingsScreen.swift       # 设置界面
├── CorewallTests/                 # 单元测试
├── docs/
│   └── site/                      # 官网源码 (zxvibe.com)
└── README.md
```

## 技术栈

| 层 | 技术 |
|---|---|
| UI | SwiftUI + AppKit (NSMenu, NSWindow) |
| 数据 | SwiftData |
| 视频 | AVFoundation (AVPlayerLayer) |
| 网页 | WKWebView (Sandboxed) |
| 购买 | StoreKit 2 |

## 架构

Corewall 采用双层渲染架构，视频层和网页层叠加在桌面之上：

```
┌──────────────────────────────────┐
│  DesktopWindow (NSWindow)        │
│  ├─ L3: AVPlayerLayer (视频)     │
│  └─ L2: WKWebView (网页)        │
├──────────────────────────────────┤
│  三级引擎生命周期                  │
│  Active (30fps)                  │
│    → Throttled (24fps, 热压力)    │
│      → AbsoluteIdle (暂停, 遮挡/低电量) │
└──────────────────────────────────┘
```

## 测试

```bash
xcodebuild test -scheme Corewall -destination 'platform=macOS'
```

## 隐私

Corewall 不收集任何个人数据。所有壁纸处理、视频渲染和配置存储均在本地完成。详见 [隐私政策](https://www.zxvibe.com/privacy.html)。

## 反馈与支持

- [GitHub Issues](https://github.com/sumachuyuan/corewall/issues) — Bug 报告和功能建议
- TestFlight App 内反馈

## 致谢

Corewall 基于开源项目 [Plash](https://github.com/sindresorhus/plash) 深度定制开发，感谢 Sindre Sorhus 及所有 Plash 贡献者。Corewall 新增了原生 AVFoundation 视频渲染层、在线画廊系统和零开销能耗管理方案。

## 许可证

Copyright © 2026 Corewall Team. All rights reserved.
