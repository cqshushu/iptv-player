# IPTV Player（手机 / TV 双端）

把 [`akiralereal/iptv`](https://github.com/akiralereal/iptv) 这个 Node.js 聚合服务端**直接装进 APK**：App 启动后在设备本地把服务跑起来（`127.0.0.1:1905`），播放器订阅它输出的 `/interface.m3u`。**无需额外服务器、无需 Termux、无需同一局域网**，装完即用。

> 本项目是 Android 客户端封装；聚合服务端的原始项目见 [akiralereal/iptv](https://github.com/akiralereal/iptv)。本仓库只发布 APK，源码构建说明见下方。

## 下载安装

请到本仓库 **[Releases](../../releases/latest)** 页面下载最新版本的两个安装包：

| 文件 | 适用设备 | 说明 |
|---|---|---|
| `IPTV-Player-<版本>-tv.apk` | 安卓电视 / 电视盒子 | TV 遥控器交互优化（OK 面板、方向键切台、信息栏） |
| `IPTV-Player-<版本>-mobile.apk` | 手机 / 平板 | 竖屏四段式列表 + 横屏全屏播放 |

- 手机和 TV 是**同一份代码**，只是文件名区分；App 内「检查更新」会按设备类型自动选对应包。
- 最低 **Android 8.0（API 26）**，仅支持 **arm64-v8a** 架构（内置 Node 运行时只有 64 位版）。
- 安装时如提示「未知来源」，请允许本应用的安装权限。

## 功能

- **内置服务**：App 内一键启动/停止本地服务（`127.0.0.1:1905`），可看日志、重启；局域网设备可访问 `http://<设备IP>:1905/admin` 管理后台（含二维码）。
- **订阅与 EPG**：支持标准 / 兼容 / 全代理三种模式；显示当前节目与「接下来」，列表可按分组筛选、搜索、收藏。
- **三播放内核**：EXO（默认）/ IJK / MPV，可随时切换；播放失败自动回退 EXO。MPV 为兼容低端 GPU 固定走软解。
- **手机端**：竖屏上半屏视频 + 下半屏「分类 / 频道 / 节目 / 线路」；横屏全屏 + OSD 进度条（回看可拖动快进）。
- **TV 端**：遥控器方向键切台、OK 唤出两列面板（分类 + 频道，右键看 EPG）、MENU 显示信息栏与进度条。
- **App 自更新**：启动自动检查本仓库 Release，发现新版本弹窗提示并下载安装；设置页可手动检查、可自定义更新地址。

## App 更新是怎么工作的

App 内置了更新清单地址：

```
https://api.github.com/repos/cqshushu/iptv-player/releases/latest
```

它读取最新 Release 的 `tag_name` 作为版本号（自动去掉开头的 `v`），并从附件里按设备类型挑 APK：TV 端选文件名含 `-tv` 的，手机端选含 `-mobile` 的。因此**每次发版务必：**

1. tag 用 `v<版本号>`，如 `v1.5.3`，且版本号要比上一个大；
2. 两个 APK 都要作为附件上传，文件名保持 `IPTV-Player-<版本>-tv.apk` / `-mobile.apk` 的命名（含 `-tv` / `-mobile` 关键词）；
3. Release 说明（正文）会原样显示在 App 的更新弹窗里，建议写清本次变更。

## 源码构建

需要 JDK 21 与 Android SDK（compileSdk 34），在工程根目录执行：

```bash
./gradlew :app:assembleRelease
```

产物在 `app/build/outputs/apk/release/`。内置服务端的打包/热更新说明见开发文档。

## 声明

- 本项目仅提供播放器技术封装，不内置任何频道源，请使用你有权访问的内容。
- 聚合服务端版权归 [akiralereal/iptv](https://github.com/akiralereal/iptv) 所有。
