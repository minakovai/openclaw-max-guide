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

## License

MIT
