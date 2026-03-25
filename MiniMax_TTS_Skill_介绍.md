> AI 语音生成工具 —— 情绪化中文 TTS

## 概述
`minimax-tts-feishu` 是一个基于 MiniMax API 的语音生成 Skill，支持将文字转换为自然的中文语音并发送到飞书。

+ **作者**: 李想 (lixiang92229)
+ **GitHub**: [https://github.com/lixiang92229/lx-minimax-tts-feishu](https://github.com/lixiang92229/lx-minimax-tts-feishu)
+ **ClawHub**: [https://clawhub.com/minimax-tts-feishu](https://clawhub.com/minimax-tts-feishu)
+ **版本**: 1.0.2
+ **协议**: MIT

---

## 功能特点

### 情绪化语音生成
根据文本内容自动检测情绪，生成带有情感色彩的语音。

**支持的的情绪**：

| 情绪 | 关键词示例 | 效果 |
| --- | --- | --- |
| happy（开心）| 太好了、哈哈、太棒了 | 明快上扬 |
| sad（悲伤）| 唉、算了、难过 | 下降低沉 |
| angry（愤怒）| 气死了、讨厌、哼 | 语速加快 |
| surprised（惊讶）| 真的吗、什么、怎么 | 语调上扬 |
| calm（平静）| 好吧、嗯、了解 | 平稳舒缓 |

### 语气词音效
自动在文本中插入音效标签，增加语音自然度：

| 标签 | 效果 |
| --- | --- |
| `(laughs)` | 笑声 |
| `(gasps)` | 倒吸气 / 惊讶 |
| `(sighs)` | 叹气 |
| `(clear-throat)` | 清嗓子 |

### 智能停顿
文本自动在标点处添加停顿：

| 标点 | 停顿时长 |
| --- | --- |
| `，` | 0.3 秒 |
| `。？！` | 0.5 秒 |

### 34+ 中文音色
内置丰富的中文音色库，支持查询和切换。

### 对话式触发
在飞书对话中直接说"转语音"，即可将最近的消息转换为语音。

---

## 工作原理

```
用户输入文字
         ↓
文本预处理
├── 情绪检测（关键词匹配）
├── 语气词音效插入
└── 停顿标记添加
         ↓
MiniMax TTS API (speech-2.8-hd 模型)
         ↓
返回 WAV 音频（16kHz 单声道）
         ↓
上传到飞书
         ↓
发送语音消息到目标用户
```

---

## 文件结构

```
minimax-tts/
├── SKILL.md              # Skill 元数据
├── README.md             # 中英双语说明
├── _meta.json            # 环境变量声明
├── index.js              # 入口文件
└── scripts/
    ├── tts_wrapper.sh      # 统一入口
    ├── tts.py              # 核心 TTS 功能
    ├── text_preprocessor.py # 文本预处理
    ├── voice_design.py     # 音色设计
    ├── list_voices.py      # 查询音色列表
    └── update_voices_map.py # 更新音色目录
```

---

## 自动日志
每次调用后自动记录：

+ **时间戳**
+ **检测情绪**
+ **文本内容**
+ **使用的音色**
+ **发送结果**

---

## 安全提示

### API 密钥保护
+ API 密钥通过环境变量传入
+ 必需变量：`MINIMAX_API_KEY`、`FEISHU_APP_ID`、`FEISHU_APP_SECRET`
+ 不要在命令行直接暴露密钥
+ 确保 `.env` 或环境变量配置安全

### 飞书凭证
+ `FEISHU_USER_OPEN_ID` 决定语音发送给谁
+ 请勿将凭证提交到公开仓库

### 网络访问
+ 本 Skill 访问 `https://api.minimaxi.com`（MiniMax TTS）
+ 本 Skill 访问 `https://open.feishu.cn`（飞书 API）

---

## 新手安装指南

### 前置要求
+ 已安装 OpenClaw
+ 已配置 MiniMax API Key
+ 已有飞书应用（App ID + App Secret）
+ 目标用户的飞书 Open ID

### 安装步骤

#### 步骤 1：确认 OpenClaw 已运行
```bash
openclaw status
```

看到类似输出说明 OpenClaw 正常运行：
```plain
Gateway:    Running
Session:    Active
```

#### 步骤 2：从 ClawHub 安装 Skill
```bash
npx clawhub install minimax-tts-feishu
```

#### 步骤 3：获取飞书应用凭证
1. 访问 [飞书开放平台](https://open.feishu.cn)
2. 创建应用（或使用现有应用）
3. 获取 `App ID` 和 `App Secret`
4. 获取目标用户的 `Open ID`

#### 步骤 4：获取 MiniMax API Key
1. 访问 [MiniMax API 平台](https://platform.minimaxi.com)
2. 注册/登录账号
3. 获取 API Key

#### 步骤 5：配置环境变量
```bash
export MINIMAX_API_KEY="your_minimax_api_key"
export FEISHU_APP_ID="your_feishu_app_id"
export FEISHU_APP_SECRET="your_feishu_app_secret"
export FEISHU_USER_OPEN_ID="target_user_open_id"
```

#### 步骤 6：验证安装
```bash
bash scripts/tts_wrapper.sh list
```

确认能看到音色列表输出。

#### 步骤 7：开始使用
```bash
# 发送测试语音
bash scripts/tts_wrapper.sh tts "你好，这是一个测试"

# 触发转语音
# 在飞书对话中发送"转语音"
```

### 常见问题

**Q: 安装后找不到命令？**  
A: 重启 OpenClaw 会话，让新 Skill 加载进来。

**Q: 语音发送失败？**  
A: 检查 `FEISHU_USER_OPEN_ID` 是否正确，目标用户是否在飞书中添加了机器人。

**Q: 如何自定义音色？**  
A: 使用 `voice_design` 命令，或在 `list` 中选择其他音色 ID。

**Q: 转语音功能怎么用？**  
A: 在飞书对话中回复某条消息并说"转语音"，即可将那条消息转为语音。

---

## 快速开始

### 安装
```bash
# 从 ClawHub 安装
npx clawhub install minimax-tts-feishu
```

### 配置
```bash
export MINIMAX_API_KEY="your_api_key"
export FEISHU_APP_ID="your_feishu_app_id"
export FEISHU_APP_SECRET="your_feishu_app_secret"
export FEISHU_USER_OPEN_ID="target_user_open_id"
```

### 基础使用示例

```bash
# 文字转语音
bash scripts/tts_wrapper.sh tts "你好，欢迎使用 TTS 语音生成"

# 指定音色
bash scripts/tts_wrapper.sh tts "你好" "female-tianmei"

# 查询可用音色
bash scripts/tts_wrapper.sh list

# 更新音色目录
bash scripts/tts_wrapper.sh update
```

### 对话式转语音

在飞书对话中：
+ **直接说"转语音"** → 将机器人最近一条消息转为语音
+ **回复某条消息说"转语音"** → 将那条消息转为语音

支持的触发词：转语音、转成语音、说一遍、语音播放

---

## 版本历史

| 版本 | 日期 | 更新内容 |
| --- | --- | --- |
| 1.0.0 | 2026-03-25 | 初始版本，支持情绪检测和语气词音效 |
| 1.0.1 | 2026-03-26 | 安全修复：移除硬编码凭证 |
| 1.0.2 | 2026-03-26 | 新增完整中英双语文档 |

---

## 写在最后

这个 Skill 源于想让 AI 语音不再机械的需求。

核心设计理念：**有情绪、有节奏、可对话**。

+ 自动检测情绪，让语音有温度
+ 语气词音效和停顿，让语音更自然
+ 对话式触发，随时转换想听的内容

希望这个工具对你也有帮助。

---

_最后更新：2026-03-26_
