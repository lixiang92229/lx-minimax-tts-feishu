# MiniMax TTS

MiniMax 文字转语音 Skill，支持中文音色、自动情绪检测、语气词音效和停顿标记。

## 功能特性

- **文字转语音**：将文本转换为自然语音发送至飞书
- **34+ 中文音色**：内置音色库，支持查询和切换
- **自动情绪检测**：根据文本关键词自动判断情绪（happy/sad/angry/surprised 等）
- **语气词音效**：自动插入笑声、叹气、惊讶等音效标签
- **智能停顿**：在标点处自动添加停顿标记，提升语音自然度
- **对话式触发**：通过"转语音"指令转换历史消息

## 安装

```bash
npx clawhub install minimax-tts-feishu
```

## 配置

需要设置以下环境变量：

| 变量 | 说明 |
|------|------|
| `MINIMAX_API_KEY` | MiniMax API Key（必填） |
| `FEISHU_APP_ID` | 飞书应用 App ID（必填） |
| `FEISHU_APP_SECRET` | 飞书应用 App Secret（必填） |
| `FEISHU_USER_OPEN_ID` | 语音发送目标用户的 Open ID（必填，用于路由） |
| `TTS_VOICES_MAP_PATH` | 音色目录路径（可选，默认 skill 内路径） |

## 使用方法

### 基础命令

```bash
# 文字转语音
/minimax-tts speak "要转换的文字"

# 查询可用音色
/minimax-tts list

# 更新音色目录
/minimax-tts update
```

### 对话式转语音

在飞书对话中输入 **"转语音"** 即可触发：

- **直接发送**"转语音" → 将我最近一条消息转成语音
- **回复某条消息**发送"转语音" → 将那条消息转成语音

支持的触发词：转语音、转成语音、说一遍、语音播放

### 手动指定音色/情绪

```
/minimax-tts speak "文字内容" --voice <voice_id> --emotion <emotion>
```

## 情绪检测

系统自动根据文本判断情绪：

| 情绪 | 关键词示例 |
|------|----------|
| happy | 开心、高兴、太好了、哈哈 |
| sad | 伤心、难过、可惜、唉 |
| angry | 生气、讨厌、哼 |
| surprised | 惊讶、真的吗、什么 |
| calm | 平静、淡定、好吧 |

## 语气词音效

自动识别的音效标签：

| 标签 | 效果 |
|------|------|
| `(laughs)` | 笑声 |
| `(gasps)` | 倒吸气/惊讶 |
| `(sighs)` | 叹气 |
| `(clear-throat)` | 清嗓子 |

## 停顿标记

自动在标点后添加停顿：

- `，` → 0.3秒
- `。？！` → 0.5秒

## 文件结构

```
minimax-tts/
├── SKILL.md              # Skill 元数据
├── README.md             # 本文档
├── index.js              # 入口文件
└── scripts/
    ├── tts.py            # 核心 TTS 功能
    ├── tts_from_chat.py  # 对话式转语音
    ├── voice_design.py   # 音色设计
    ├── list_voices.py    # 查询音色列表
    ├── update_voices_map.py  # 更新音色目录
    ├── text_preprocessor.py  # 文本预处理
    └── tts_wrapper.sh    # 统一入口
```

## 默认音色

**Chinese (Mandarin)_Gentle_Senior（温柔学姐）** - 温柔、知性、略带亲切感的声音

## 更新日志

### 1.0.0
- 初始版本
- 支持文字转语音
- 支持 34+ 中文音色
- 支持自动情绪检测
- 支持语气词音效
- 支持对话式转语音触发
