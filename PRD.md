# KidSudoku — Product Requirements Document

## Overview

KidSudoku 是一款面向 3-6 岁儿童的数独 + Phonics 学习 App，通过数独闯关驱动 Jolly Phonics 音素学习，以抽卡收集机制激励持续游玩。

## Platform & Distribution

- **Framework:** Flutter
- **Target Platform:** Amazon Appstore (Fire OS / Kindle Fire tablets)
- **Price:** Paid ($2.99)
- **Category:** Kids (3-6 years)
- **Compliance:** COPPA compliant — no ads, no tracking, no in-app purchases

---

## Core Gameplay

### Sudoku

| 属性 | 说明 |
|---|---|
| 阶数 | 4×4, 6×6 |
| 难度 | 3 级: Easy / Medium / Hard |
| 重置 | 一键重置当前关卡 |
| 提示 | 无 |
| 计时器 | 有，记录完成时间 |
| 生成 | 算法动态生成，保证唯一解 |

### 难度与 Phonics 组同步进阶

| Phonics Group | 数独类型 | 难度 |
|---|---|---|
| Group 1 (s, a, t, i, p, n) | 4×4 | Easy |
| Group 2 (ck, e, h, r, m, d) | 4×4 | Medium |
| Group 3 (g, o, u, l, f, b) | 4×4 | Hard |
| Group 4 (ai, j, oa, ie, ee, or) | 6×6 | Easy |
| Group 5 (z, w, ng, v, oo, oo) | 6×6 | Medium |
| Group 6 (y, x, ch, sh, th, th) | 6×6 | Hard |
| Group 7 (qu, ou, oi, ue, er, ar) | 6×6 | Hard+ |

> Group 7 可作为额外挑战关，或调整映射关系。

---

## Phonics + Collection System

### Jolly Phonics 框架

遵循 Jolly Phonics 教学顺序，共 7 组 42 个音素。

### Collection（抽卡系统）

- 每个 Phonics Group = 1 个 Collection
- 每个 Collection 含 **18 张卡片**（每个音素 3 个对应单词）
- **无固定关卡，动态生成无限数独题目**
- 完成一关数独 → 可进行 **1 次抽卡**
- 抽卡 **可重复**（集齐需要运气和持续游玩）
- 卡片内容：原创卡通插画 + 单词 + 音素标注

### 卡片交互

- **点击卡片** → 播放该单词的发音（本地音频资源，离线可用）
- 音频固定打包在 App 内

### 展柜（Achievement Display）

- 每个 Collection 有独立展柜页面
- 展示已收集 / 未收集卡片（未收集显示为剪影或问号）
- 显示收集进度（如 12/18）
- 集齐一个 Collection 有完成动画/特效

---

## UI/UX Design Direction

- **风格:** 色彩鲜艳、卡通风格，适合幼儿
- **角色:** 原创卡通形象（避免任何版权 IP）
- **语言:** 英文为主（Phonics 内容为英文）
- **交互:** 大按钮、简单手势，适合小手操作
- **离线:** 完全离线可玩

---

## Content Inventory

### 音频资源

- 42 个音素发音
- 18 × 7 = **126 个单词发音**
- 格式：MP3 或 OGG，打包在 App 内

### 插画资源

- **126 张卡片插画**（每张对应一个单词的卡通形象）
- 7 个 Collection 封面
- UI 元素（按钮、背景、展柜框架等）

### 数独内容

- 算法动态生成，无需预制题库
- 保证每题唯一解

---

## Technical Architecture

```
lib/
├── main.dart
├── models/
│   ├── sudoku.dart          # 数独数据模型
│   ├── card.dart            # 卡片数据模型
│   ├── collection.dart      # Collection 数据模型
│   └── progress.dart        # 用户进度模型
├── services/
│   ├── sudoku_generator.dart  # 数独生成算法
│   ├── audio_service.dart     # 音频播放
│   └── storage_service.dart   # 本地存储 (SharedPreferences)
├── screens/
│   ├── home_screen.dart       # 主页
│   ├── collection_screen.dart # Collection 选择
│   ├── sudoku_screen.dart     # 数独游戏页
│   ├── gacha_screen.dart      # 抽卡动画页
│   └── showcase_screen.dart   # 展柜页
├── widgets/
│   ├── sudoku_grid.dart       # 数独网格组件
│   ├── card_widget.dart       # 卡片组件
│   ├── timer_widget.dart      # 计时器组件
│   └── number_pad.dart        # 数字输入键盘
└── utils/
    └── constants.dart         # 常量定义
```

### 本地存储

- 使用 SharedPreferences 或 Hive 存储：
  - 用户进度（已完成关卡）
  - 已收集卡片
  - 最佳时间记录

### Fire OS 适配

- 移除所有 Google Play Services 依赖
- 使用 Amazon Appstore 提交流程
- 适配 Kindle Fire 屏幕尺寸

---

## User Flow

```
启动 App
  → 主页（显示 7 个 Collection 入口）
    → 选择 Collection (如 Group 1)
      → 显示关卡列表（按难度排列）
        → 选择关卡 → 进入数独游戏
          → 完成数独 ✅
            → 抽卡动画 🎴
              → 获得卡片（展示单词 + 播放发音）
                → 返回关卡列表 / 查看展柜
```

---

## Milestones

| 阶段 | 内容 | 预估 |
|---|---|---|
| M1 | Flutter 项目搭建 + 数独生成算法 + 基础 UI | 1 week |
| M2 | 数独游戏页完整功能（计时、重置、输入） | 1 week |
| M3 | Collection 系统 + 抽卡机制 + 展柜 | 1 week |
| M4 | Phonics 音频集成 + 卡片发音交互 | 1 week |
| M5 | 插画资源制作 + UI 美化 | 2 weeks |
| M6 | Fire OS 适配 + 测试 + Amazon 提交 | 1 week |

---

## Open Questions

1. **插画资源制作** — 126 张卡片插画谁来做？AI 生成 + 人工调整？外包？
2. **音频资源** — TTS 生成还是真人录制？儿童向建议真人录制
3. **App 名称** — "KidSudoku" 还是更有品牌感的名字？
4. **隐私政策** — Kids 类目需要隐私政策页面，需要准备
5. **Amazon 开发者账号** — 需要注册（$0 免费）
