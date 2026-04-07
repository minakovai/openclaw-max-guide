# MAX Audio Skill

Скилл для обработки голосовых сообщений в MAX мессенджере.

## Описание

Автоматически распознаёт голосовые сообщения, полученные от пользователей MAX, и преобразует их в текст для дальнейшей обработки AI-моделью.

## Требования

```bash
pip install openai-whisper
```

## Использование

Скилл работает автоматически при получении голосового сообщения в MAX.

### Ручной вызов (если нужно)

```bash
whisper --model base --language ru --fp16 False --output_format txt --output_dir /tmp/openclaw-stt /path/to/audio.ogg
```

## Параметры

- `--model` — модель Whisper (`tiny`, `base`, `small`, `medium`, `large`)
- `--language` — язык (`ru`, `en`, `auto`)
- `--fp16 False` — отключение half-precision (рекомендуется для CPU)

## Интеграция с OpenClaw

Добавьте в `openclaw.json`:

```json
{
  "tools": {
    "media": {
      "audio": {
        "enabled": true,
        "maxBytes": 20971520,
        "models": [
          {
            "type": "cli",
            "command": "whisper",
            "args": [
              "--model", "base",
              "--language", "ru",
              "--fp16", "False",
              "--output_format", "txt",
              "--output_dir", "/tmp/openclaw-stt",
              "{{MediaPath}}"
            ],
            "timeoutSeconds": 120
          }
        ],
        "echoTranscript": false
      }
    }
  }
}
```

## Лицензия

MIT
