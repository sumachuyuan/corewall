<div align="center">
  <h1>Corewall</h1>
  <p><b>macOS 沉浸式桌面体验 — 网页、视频、白噪音，想放什么放什么</b></p>
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
  <img src="https://img.shields.io/badge/Swift-6.0-orange?logo=swift" alt="Swift">
</p>

---

## 预览

<p align="center">
  <img src="assets/img/01-discover-wallpapers.webp" width="45%" alt="精选图库" />
  <img src="assets/img/02-discover-wallpapers.webp" width="45%" alt="精选图库" />
</p>
<p align="center">
  <img src="assets/img/03-discover-wallpapers.webp" width="45%" alt="精选图库" />
  <img src="assets/img/04-web-wallpapers.webp" width="45%" alt="网页桌面" />
</p>
<p align="center">
  <img src="assets/img/05-lucky-drop.webp" width="45%" alt="Lucky Drop" />
</p>

---

## 功能亮点

- **网页桌面** — 粘个网址，桌面就是网页。支持 WebGL、Canvas 和 3D 交互式网页。内置白噪音、时间等精选网址，后台可随时添加新内容
- **视频桌面** — 基于 AVFoundation 优化的渲染引擎，极低 CPU 占用即可流畅播放 4K 视频，带声音
- **Lucky Drop** — 盲盒开箱，惊喜抽取。每次切换都有新发现
- **精选图库** — 海量图片提供挑选，ACG、赛博朋克、自然风景等分类。不喜欢可以直接去掉，不会再出现
- **本地导入** — 自己的视频和图片也能用
- **电池保护** — 检测电池电量，低电量自动暂停以节省续航
- **热量管理** — 三级引擎生命周期（active / throttled / absoluteIdle），根据系统温度自动调节
- **多显示器** — 每个屏幕独立设置，支持 Space 切换
- **菜单栏控制** — 一键切换、暂停、恢复，无需打开主窗口

## 系统要求

- macOS 14.0 (Sonoma) 或更高版本
- Apple Silicon / Intel 均支持

## 安装

### App Store

前往 [App Store](https://apps.apple.com/app/id6762517792) 下载正式版。

### TestFlight

加入 [TestFlight 公开测试](https://testflight.apple.com/join/8hBCbycu) 获取最新版本。

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
│   ├── WallpaperAsset.swift       # 数据模型 (SwiftData)
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
│  Active (60fps)                  │
│    → Throttled (热压力降频)       │
│      → AbsoluteIdle (遮挡/低电量) │
└──────────────────────────────────┘
```

## 测试

```bash
xcodebuild test -scheme Corewall -destination 'platform=macOS'
```

## 隐私

Corewall 不收集任何个人数据。所有处理、渲染和配置存储均在本地完成。详见 [隐私政策](https://www.zxvibe.com/privacy.html)。

## 反馈与联系

- [GitHub Issues](https://github.com/sumachuyuan/corewall/issues) — Bug 报告和功能建议
- [邮箱](mailto:tidilist@gmail.com) — 私密问题
- [官网](https://zxvibe.com) — 了解更多

## 致谢

Corewall 的早期灵感来自 [Plash](https://github.com/sindresorhus/Plash)，感谢 Sindre Sorhus 及所有 Plash 贡献者。Corewall 使用了以下开源组件：[Defaults](https://github.com/sindresorhus/Defaults)、[LaunchAtLogin-Modern](https://github.com/sindresorhus/LaunchAtLogin-Modern)、[KeyboardShortcuts](https://github.com/sindresorhus/KeyboardShortcuts)。

## 许可证

Copyright © 2026 Corewall Team. All rights reserved.
