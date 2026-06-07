<p><br></p>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="public/salvation_lies_within_IELTS_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="public/salvation_lies_within_IELTS_light.svg">
  <img alt="Slogan: Salvation lies within IELTS" src="public/salvation_lies_within_IELTS_light.svg">
</picture>

<p><br></p>
<p><br></p>
<h1 align='center'>
  My <span>IELTS</span>
</h1>

<h2>在线地址 <a href="https://troyesu.github.io/my-ielts/#/">https://troyesu.github.io/my-ielts/</a></h2>

> 本项目 Fork 自 [hefengxian/my-ielts](https://github.com/hefengxian/my-ielts)，感谢原作者和所有贡献者。

## Troye 的改动

基于原项目进行了大量扩展和优化：

### 听力
- 新增 **听力真题语料库**（9 个 Chapter，67+ 小节），含拼写规范、吞音连读、特别名词、同义替换等
- 语料库支持章节内子导航，数据包含中英对照、词性标注
- 生成 **4000+ TTS 音频文件**（英音 en-GB-SoniaNeural）

### 阅读
- 新增 **阅读 538 考点词练习**页面，支持根据中文提示输入考点词
- 修复练习按钮错误跳转到听力页面的 bug

### 写作
- 新增 **雅思小作文（Task 1）**模块：评分标准、6 种题型、趋势词汇表、高频句型、地图题词汇
- 新增 **雅思大作文（Task 2）**模块：4 种题型框架、8 大话题词汇、衔接语、审题方法
- 100 句翻译练习保留

### 词汇
- 单词打字练习新增 **本章随机 / 全库随机** 模式
- 去除行颜色交替，优化文字层级

### 口语
- 新增口语框架页面：Part 1/2/3 逻辑链（ARE/OREC）、高频话题清单、衔接语、备考建议

### UI 优化
- 统一"索引"列名，中文/词性文字颜色降淡
- 主页文字居中，深色模式适配

---

## 概述

雅思备考资料，包含词汇、语法、听说读写最出名的一些内容

- [x] 词汇练习模式
- [x] 词汇打字练习（顺序/本章随机/全库随机）
- [x] 听力语料库
- [x] 阅读 538 考点词练习
- [x] 写作 Task 1 & Task 2 框架
- [x] 口语框架

## 规划栏目

### 词汇

> 2026-03 增加打字练习模式，感谢 [@Tommy1109255](https://github.com/Tommy1109255)

雅思词汇真经（刘洪波橙色的那本）

- 雅思核心词汇
- 逻辑词群记忆法
- 原书音频

### 语法

新东方雅思语法

- 视频
- 讲义
- 思维导图

### 听力

- 基本概念和应试技巧
- 听力 179 考点词 + 练习
- 听力真题语料库

### 口语

ARE/OREC 逻辑链 · Part 1/2/3 框架 · 高频话题 · 衔接语

### 阅读

- 538 考点词同义替换
- 538 考点词练习

### 写作

- 100 句翻译练习（顾家北）
- 雅思大作文框架
- 雅思小作文框架

## 开发

本项目基于

- [Vitesse Lite](https://github.com/antfu/vitesse-lite) 模板
- 样式部分参照了 [Flowbite](https://github.com/themesberg/flowbite) & [Flowbite Admin Dashboard](https://flowbite-admin-dashboard.vercel.app)

技术栈：Vue 3 + Vite + UnoCSS + pnpm + TypeScript

```bash
# 安装依赖
pnpm i

# 开发模式
pnpm run dev

# 构建
pnpm run build
```

## 禁止将本项目用于任何商业目的！！！
