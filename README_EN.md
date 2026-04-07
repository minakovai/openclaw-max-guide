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
