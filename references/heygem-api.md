# HeyGem API 调用指南

---

## 本地 API 端点

| 端点 | 用途 |
|------|------|
| `http://127.0.0.1:8383/health` | 健康检查 |
| `http://127.0.0.1:8383/easy/submit` | 提交视频合成任务 |
| `http://127.0.0.1:18180/v1/voices` | 获取可用音色列表 |
| `http://127.0.0.1:18180/v1/clone` | 克隆声音 |
| `http://127.0.0.1:18180/v1/invoke` | TTS 语音合成（音色试听用） |

---

## 提交视频合成任务

```bash
curl -X POST http://127.0.0.1:8383/easy/submit \
  -F "image_path=/path/to/photo.jpg" \
  -F "text=大家好，今天推荐一款..." \
  -F "voice=温柔女声"
```

参数：

| 参数 | 说明 |
|------|------|
| image_path | 照片/视频路径 |
| text | 文案内容 |
| voice | "default" / 预置音色名 / 克隆音色 ID |
| duration | 可选，限制视频时长（秒） |

返回：

```json
{ "task_id": "xxx", "status": "processing" }
```

---

## 轮询进度

```bash
curl "http://127.0.0.1:8383/easy/status?task_id=xxx"
```

返回：

```json
{ "status": "done", "output_path": "/output/video.mp4" }
```

status 值：`processing` / `done` / `failed`

---

## 获取音色列表

```bash
curl http://127.0.0.1:18180/v1/voices
```

返回预置音色列表 + 克隆音色列表。

---

## 克隆声音

```bash
curl -X POST http://127.0.0.1:18180/v1/clone \
  -F "audio=@sample.mp3" \
  -F "name=我的声音"
```

返回：

```json
{ "voice_id": "cloned_xxx", "name": "我的声音" }
```

---

## TTS 试听

```bash
curl -X POST http://127.0.0.1:18180/v1/invoke \
  -H "Content-Type: application/json" \
  -d '{"text":"你好，这是音色试听效果","voice":"温柔女声"}'
```

返回音频文件（mp3/wav）。

---

## 字幕生成（ffmpeg）

```bash
ffmpeg -i video.mp4 -vf "subtitles=sub.srt:force_style='FontSize=24,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000'" output.mp4
```

SRT 按 4字/秒估算时长自动切分。

---

## 云端 API

当你部署到服务器后，本地 API 可替换为云端地址，参数相同，加上 API Key 认证：

```bash
curl -X POST https://your-server.com/submit \
  -H "Authorization: Bearer {api_key}" \
  -F "image=@photo.jpg" \
  -F "text=..." \
  -F "voice=..."
```
