PRD - 浏览器端 NES 模拟器
1. 背景与目标

NES（任天堂红白机）是经典的 8 位游戏机，拥有大量怀旧游戏。
本项目目标是用 TypeScript + React 在浏览器端实现 NES 模拟器，支持 UI 与逻辑分离，使用 pnpm workspace 做 mono-repo 架构，并提供：

基础模拟器功能（运行 ROM、输入输出、音视频）

调试工具（适合学习与开发者）

金手指（Cheats） 支持

一些差异化的特色功能（如存档、回放、Shader 效果）

2. 用户画像

普通玩家
想通过浏览器玩 NES 游戏，支持金手指、快速存档。

开发者 / 学习者
希望通过调试功能研究 6502 CPU、PPU 渲染等机制。

速通爱好者
需要录像、回放、Rewind、状态存档。

3. 功能需求
3.1 核心功能

支持加载 NES ROM（iNES 格式）。

CPU（6502）、PPU、APU、Mapper 的模拟。

游戏画面渲染到 Canvas（支持 pixelated 像素风格）。

音频输出（WebAudio / AudioWorklet）。

手柄输入（键盘映射，后续扩展为手柄 API）。

Save/Load state（快速存档与读取）。

3.2 调试功能

运行控制：暂停 / 恢复 / 单步执行（按指令 / 按帧）。

CPU 状态显示：寄存器（A、X、Y、SP、PC、P）。

断点：

执行断点（指定地址 PC 命中）。

内存断点（读/写某地址触发）。

反汇编器：显示指定地址区间的指令。

内存查看与修改。

日志与 Trace：记录指令执行历史。

可视化：PPU 状态（如 NameTable、Sprite、调色板）。

3.3 金手指功能

支持 Game Genie 码 的输入与解析。

Cheat 管理（新增 / 删除 / 开关）。

Cheat 类型：

写入一次：启动时写入内存。

冻结值：持续强制写入，防止变量改变。

条件写入：符合条件才修改。

Cheat 保存到 localStorage（下次自动加载）。

3.4 特色功能

Rewind（回溯）：保存最近 N 秒状态，允许倒退。

录像回放：记录玩家输入，支持回放分享。

画面滤镜 / Shader：CRT 扫描线、像素柔化。

多存档槽位：不同游戏/场景快速切换。

ROM 管理器：展示游戏封面 / 元数据（需用户上传，不提供 ROM）。

4. 非功能需求

性能：60 FPS 稳定运行，音视频同步。

兼容性：支持主流浏览器（Chrome/Edge/Firefox/Safari），桌面端优先，移动端可降级。

可扩展性：采用 mono-repo，UI 与逻辑分离，方便后续引入 Wasm 或 Node.js 版本。

安全性：ROM、存档、Cheat 数据保存在浏览器本地（IndexedDB/localStorage）。

5. 技术架构与项目结构
5.1 技术选型

语言：TypeScript

UI 框架：React + Tailwind（UI 简洁）

构建工具：Vite

包管理：pnpm workspace（mono-repo）

音频：WebAudio / AudioWorklet

渲染：Canvas / OffscreenCanvas

5.2 Mono-repo 结构
/packages
  /core         -> NES 仿真核心（CPU/PPU/APU/Mapper）
  /worker       -> 封装 core 到 Web Worker
  /debugger     -> 调试协议与工具
  /cheats       -> 金手指解析与管理
  /common-types -> 通用类型定义
/apps
  /app-web      -> React UI（主应用）

6. 里程碑
M1 - 核心运行

CPU/PPU/APU 基本实现

能运行简单 ROM，输出画面 + 声音

基本输入支持

M2 - 基础功能

Save/Load state

Web Worker 架构完成

React UI 最小可运行版

M3 - 调试工具

暂停/恢复/单步

CPU 状态显示

内存查看/修改

断点与反汇编

M4 - 金手指

Game Genie 解码

Cheat 管理 UI

Freeze / 条件写入

M5 - 特色功能

Rewind

回放录像

CRT Shader 效果

ROM 管理器

M6 - 优化与发布

性能优化（音视频同步）

跨浏览器兼容

文档与 Demo 发布