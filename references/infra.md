# Twilio + AWS + OpenAI 語音基礎設施

## 環境變數

| 變數名 | 說明 |
|--------|------|
| `TWILIO_ACCOUNT_SID` | Twilio Account SID |
| `TWILIO_API_KEY` | Twilio API Key SID |
| `TWILIO_API_SECRET` | Twilio API Secret |
| `TWILIO_FROM_NUMBER` | Twilio 撥出號碼（E.164 格式） |
| `OPENAI_TTS_KEY` | OpenAI API Key（用於 TTS） |

AWS Transcribe 使用 pod 內的 AWS credentials（IAM role 或 env）。

## 語音合成 (TTS)
- **服務：** OpenAI TTS
- **Model：** tts-1
- **Voice：** nova（年輕女聲，溫暖自然）
- **輸出格式：** pcm（24kHz 16-bit，程式內降頻至 8kHz）

## 語音辨識 (STT)
- **服務：** AWS Transcribe Streaming
- **Language：** zh-TW
- **MediaEncoding：** pcm
- **SampleRate：** 8000 Hz（Twilio Media Streams 原生格式轉換後）

### 已知限制
- 中文短句辨識率偏低（電話音質 + 短回答）
- 名字容易被誤辨識為同音詞

## 依賴套件
```bash
cd /tmp && npm install ws @aws-sdk/client-transcribe-streaming
```

## 音訊格式
- Twilio Media Streams：mulaw 8kHz
- OpenAI TTS 輸出：PCM 24kHz 16-bit → 程式內降頻至 8kHz → mulaw
- Transcribe 輸入：PCM 8kHz 16-bit（從 mulaw 轉換）
