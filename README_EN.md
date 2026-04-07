# OpenClaw + MAX Integration Guide

Complete guide for integrating OpenClaw with MAX messenger (max.ru).

## Quick Start

```bash
openclaw plugins install @olegbalbekov/openclaw-max
```

## Configuration

Add to `~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "max": {
      "enabled": true,
      "token": "YOUR_MAX_BOT_TOKEN",
      "dmPolicy": "allowlist",
      "allowFrom": ["YOUR_USER_ID"]
    }
  }
}
```

See [examples/](examples/) for more configuration examples.

## Documentation

- [Russian README](README.md)
- [Original Plugin](https://github.com/olegbalbekov/openclaw-max)

## Media Support (Images & Files)

MAX plugin supports sending and receiving images.

### Receiving Images

When user sends an image in MAX:
1. OpenClaw automatically downloads the image
2. AI can analyze it (if model supports vision)
3. Response is sent back to user

### Sending Images

Use `MEDIA:` prefix in responses:
```
MEDIA:https://example.com/image.jpg
Your text response
```

See [skills/max-media/](skills/max-media/) for details.

---

## Voice Messages

MAX supports voice messages. OpenClaw uses OpenAI Whisper for transcription.

### Setup

```bash
pip install openai-whisper
```

### How it works

1. User sends voice message in MAX
2. OpenClaw downloads audio automatically
3. Whisper transcribes to text
4. AI model processes the text
5. Response sent back to user

See [skills/max-audio/](skills/max-audio/) for skill configuration.

## License

MIT
