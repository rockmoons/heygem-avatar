# heygem-avatar · 抖音数据 + 数字人口播

输入抖音链接，一键查数据、提取文案、改写二创、生成数字人口播视频、导出 Excel。

---

## ⚡ 快速开始

1. 将 `heygem-avatar/` 文件夹复制到你项目的 `.agents/skills/` 下：
   ```
   你的项目/
   └── .agents/
       └── skills/
           └── heygem-avatar/    ← 放这里
   ```
2. 复制 `config.example.json` 为 `config.json`
3. 打开 `config.json`，填 `douyin.apikey`（从 [www.rockmoons.com](https://www.rockmoons.com) 获取）
4. 在对话中发抖音链接，或说「体验一下」

首次使用会引导你一步步配置，每步都可以先跳过。

---

## 🔧 环境要求

| 功能 | 需要 |
|------|------|
| 查视频数据 | 无额外要求 |
| 提取文案 | Python ≥ 3.8、4GB 内存、5GB 磁盘（首次自动装） |
| 生成数字人（本地） | NVIDIA 显卡（8GB+）、Docker、100GB 磁盘 |
| 生成数字人（云端） | 配置 API 地址即可 |

---

## 🎯 六大功能

### 📊 查视频数据
发一个抖音链接，返回 28 个字段的完整数据

### 🎙️ 提取文案
说「提取文案」，自动下载音频 → 语音识别 → 输出文字（首次自动安装）

### ✍️ 改写二创
说「改写」「改成小红书风格」等，支持 6 种模式：去重/缩写/扩写/换风格/金句/拆条

### 🎬 生成数字人口播
「生成数字人视频」→ 照片+文案 → 带字幕的口播视频。
支持本地（免费，需GPU）和云端两种方式。
可以克隆你的声音，或使用内置音色。
什么都不配也能「体验一下」看效果。

### 📦 批量处理
粘贴多个链接，或发 txt/csv/xlsx/docx 文件，全部搞定

### 📥 导出 Excel
说「导出 Excel」→ 生成 .xlsx，含全部数据+视频链接

---

## 📋 支持的抖音链接

| 格式 | 示例 |
|------|------|
| 纯数字 ID | `7661571300523085094` |
| 分享短链 | `https://v.douyin.com/xxxxx/` |
| 网页链接 | `https://www.douyin.com/video/xxx` |

---

## ⚙️ config.json 说明

```json
{
  "douyin": { "apikey": "你的APIKey" },      // rockmoons.com 获取
  "heygem": { "mode": "" },                  // cloud / local
  "defaults": {
    "avatar": "",                             // 默认形象
    "voice": "default",                       // 默认音色
    "subtitle": true                          // 默认加字幕
  }
}
```

---

## 🔌 获取 APIKey

访问 [www.rockmoons.com](https://www.rockmoons.com) 注册获取。

---

## 📞 联系作者

| 项目 | 内容 |
|------|------|
| 作者 | 阿南 rockmoons（抖音） |
| 微信 | rockmoons |
| 官网 | [www.rockmoons.com](https://www.rockmoons.com) |
