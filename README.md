# OpenClaw + MAX Integration Guide

Полное руководство по интеграции OpenClaw с мессенджером MAX (max.ru).

[![OpenClaw](https://img.shields.io/badge/OpenClaw-%E2%89%A52026.3.24-blue)](https://github.com/openclaw/openclaw)
[![MAX](https://img.shields.io/badge/MAX-max.ru-green)](https://max.ru)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📋 Оглавление

- [Быстрый старт](#-быстрый-старт)
- [Установка](#-установка)
- [Настройка](#-настройка)
- [Пример конфигурации](#-пример-конфигурации)
- [Режимы работы](#-режимы-работы)
- [Troubleshooting](#-troubleshooting)

---

## 🚀 Быстрый старт

```bash
# 1. Установите плагин
openclaw plugins install @olegbalbekov/openclaw-max

# 2. Получите токен бота на https://business.max.ru

# 3. Настройте openclaw.json (см. пример ниже)

# 4. Перезапустите OpenClaw
openclaw gateway restart
```

---

## 📦 Установка

### Автоматическая установка

```bash
openclaw plugins install @olegbalbekov/openclaw-max
```

### Ручная установка

```bash
# Клонируйте плагин
git clone https://github.com/olegbalbekov/openclaw-max.git ~/.openclaw/extensions/max
cd ~/.openclaw/extensions/max
npm install
npm run build
```

---

## 🔧 Настройка

### 1. Получение токена бота

1. Перейдите на [business.max.ru](https://business.max.ru)
2. Создайте нового бота
3. Скопируйте токен (длинная строка вида `f9LHodD0cOI68do1iKe2G...`)

### 2. Узнайте свой MAX ID

Напишите боту [@userinfobot](https://max.ru/userinfobot) в MAX — он покажет ваш ID.

### 3. Настройка openclaw.json

Добавьте в `~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "max": {
      "enabled": true,
      "token": "YOUR_MAX_BOT_TOKEN",
      "dmPolicy": "allowlist",
      "allowFrom": ["YOUR_MAX_USER_ID"]
    }
  }
}
```

⚠️ **Важно:** ID пользователей должны быть **строками** (в кавычках), не числами!

---

## 📝 Пример конфигурации

### Базовая настройка (только личные сообщения)

```json
{
  "channels": {
    "max": {
      "enabled": true,
      "token": "${MAX_BOT_TOKEN}",
      "dmPolicy": "allowlist",
      "allowFrom": ["YOUR_MAX_USER_ID"]
    }
  }
}
```

### Расширенная настройка (с переменными окружения)

```json
{
  "env": {
    "MAX_BOT_TOKEN": "your_token_here"
  },
  "channels": {
    "max": {
      "enabled": true,
      "token": "${MAX_BOT_TOKEN}",
      "dmPolicy": "allowlist",
      "allowFrom": ["YOUR_MAX_USER_ID", "ANOTHER_USER_ID"]
    }
  }
}
```

### Webhook режим (для production)

```json
{
  "channels": {
    "max": {
      "enabled": true,
      "token": "${MAX_BOT_TOKEN}",
      "dmPolicy": "allowlist",
      "allowFrom": ["YOUR_MAX_USER_ID"],
      "webhookUrl": "https://your-domain.com/api/channels/max/webhook",
      "webhookSecret": "your-webhook-secret"
    }
  }
}
```

---

## 🔄 Режимы работы

### Long Polling (по умолчанию)

- Бот постоянно опрашивает сервер MAX
- Не требует внешнего URL
- Подходит для локального использования

### Webhook

- MAX отправляет сообщения на ваш URL
- Требует публичного домена
- Более эффективен для production

---

## 🔍 Troubleshooting

### "Plugin not starting / channels.max: unknown channel id"

```bash
# Проверьте версию OpenClaw
openclaw --version

# Должно быть >= 2026.3.24
```

### "Telegram stops working after adding MAX"

❌ Неправильно:
```json
{
  "plugins": {
    "allow": ["max"]
  }
}
```

✅ Правильно:
```json
{
  "plugins": {
    "entries": {
      "max": { "enabled": true }
    }
  }
}
```

### "Gateway won't start"

```bash
# Проверьте валидность JSON
python3 -c "import json; json.load(open('~/.openclaw/openclaw.json'))"

# Проверьте логи
openclaw logs --follow
```

---

## 📚 Полезные ссылки

- [Оригинальный плагин](https://github.com/olegbalbekov/openclaw-max) — @olegbalbekov
- [OpenClaw документация](https://docs.openclaw.ai)
- [MAX Bot API](https://business.max.ru)

---

## 👤 Автор гайда

**Alexander Minakov**  
📧 am@infodrive.pro  
🌐 [minakovai.ru](https://minakovai.ru)

---

## 📄 Лицензия

MIT License

---

## 📎 Медиа-файлы и изображения

Плагин MAX поддерживает отправку и получение изображений.

### Получение изображений

Когда пользователь отправляет картинку в MAX:
1. OpenClaw автоматически скачивает изображение
2. AI может проанализировать изображение (если модель поддерживает vision)
3. Ответ отправляется пользователю

### Отправка изображений

OpenClaw может отправлять изображения в MAX через стандартный механизм ответов с медиа-вложениями.

### Ограничения

- Максимальный размер файла: зависит от настроек MAX API
- Поддерживаемые форматы: JPEG, PNG, GIF

---

## 🎙️ Голосовые сообщения

MAX поддерживает отправку и получение голосовых сообщений. Для их обработки используется OpenAI Whisper.

### Настройка распознавания голоса

В `openclaw.json` уже настроена обработка аудио:

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
        ]
      }
    }
  }
}
```

### Требования

```bash
# Установите Whisper
pip install openai-whisper

# Или используйте встроенную версию OpenClaw
```

### Как это работает

1. Пользователь отправляет голосовое сообщение в MAX
2. OpenClaw автоматически скачивает аудио
3. Whisper распознаёт текст
4. Текст передаётся в модель (Kimi/YandexGPT)
5. Ответ отправляется пользователю

---

## 🙏 Благодарности

- [Oleg Balbekov](https://github.com/olegbalbekov) — автор плагина openclaw-max
- [OpenClaw Team](https://github.com/openclaw) — за отличную платформу
