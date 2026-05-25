# 2030ai-dev-environment-setup (macOS)

Руководство по настройке окружения для работы с Claude Code + MCP + Superpowers на macOS.

GitHub slug: `2030ai/2030ai-dev-environment-setup`.

> **Версия для Windows:** [dev-environment-setup-windows](https://github.com/Afanaseva/dev-environment-setup-windows)

---

## Содержание

1. [Cursor](#1-cursor)
2. [Python](#2-python)
3. [Node.js](#3-nodejs)
4. [Claude Code](#4-claude-code)
5. [Авторизация в Claude](#5-авторизация-в-claude)
6. [Homebrew](#6-homebrew)
7. [Git и GitHub CLI](#7-git-и-github-cli)
8. [MCP-плагины](#8-mcp-плагины)
9. [Superpowers Skills](#9-superpowers-skills)
10. [Проверка окружения](#10-проверка-окружения)
11. [Автоматическая установка (промпт)](#11-автоматическая-установка-промпт)

---

## 1. Cursor

### Шаг 1. Скачать

Перейти на официальный сайт: https://cursor.sh

Нажать **Download for Mac**.

### Шаг 2. Установить

- Открыть скачанный `.dmg`
- Перетащить Cursor в папку **Applications**
- Запустить Cursor из Applications

**Если macOS ругается:**

`System Settings → Privacy & Security → Allow Anyway`

---

## 2. Python

### Шаг 1. Проверить установку

Открываем в Cursor: `Terminal → New Terminal`

Вводим в терминале:
```bash
python3 --version
```

Или просто спрашиваем у Курсора какой Python установлен.

Если версия показывается (например `3.11.x`) — переходите к следующему разделу.

### Шаг 2. Если Python НЕ установлен

1. Переходим на официальный сайт: https://www.python.org/downloads/mac-osx/
2. Скачиваем последнюю версию Python 3
3. Устанавливаем обычным способом (Next → Next)
4. После установки перезапускаем Cursor и снова проверяем:

```bash
python3 --version
```

---

## 3. Node.js

Node.js нужен для Claude Code.

### Установка

1. Переходим на: https://nodejs.org
2. Скачиваем **LTS версию** для macOS
3. Устанавливаем

### Проверка

```bash
node -v
npm -v
```

Или просто спрашиваем у Курсора установлен ли Node.js.

---

## 4. Claude Code

В терминале Cursor вводим:

```bash
npm install -g @anthropic-ai/claude-code
```

Проверяем:

```bash
claude --version
```

Если версия отображается — всё работает.

---

## 5. Авторизация в Claude

### Порядок действий

0. **Включить VPN**
1. Открыть Cursor
2. **ЗАКРЫТЬ окно диалога с Cursor (панель справа) — насовсем**
3. Открыть терминал
4. В терминале написать:

```bash
claude
```

5. Claude спросит, есть ли у вас оплаченный аккаунт и предложит пронумерованные варианты. Под вариантом **номер 1** будет нужный вариант (что подписка есть) — нажмите цифру `1`.
6. Вас перекинет в браузер для авторизации.
7. Если в терминале появился оранжевый рисунок с приветствием — всё OK!

### Если не вышло

**А)** Убедиться, что VPN включен и работает

**Б)** Если требует залогиниться заново — вводим в терминале:

```
/login
```

и проходим авторизацию заново.

---

## 6. Homebrew

Homebrew — пакетный менеджер для macOS.

### Проверка

```bash
brew --version
```

### Если не установлен

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

После установки добавьте в PATH (для zsh):

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```

Обновите:

```bash
brew update
```

---

## 7. Git и GitHub CLI

### Установка через Homebrew

```bash
brew install git gh
```

### Проверка

```bash
git --version
gh --version
```

> **Примечание:** GitHub CLI (gh) устанавливается, но авторизация в GitHub не требуется на этом этапе.

---

## 8. MCP-плагины

MCP-плагины расширяют возможности Claude Code.

### A) Playwright MCP

```bash
claude mcp add --scope user playwright -- npx -y @playwright/mcp@latest
```

Проверка:

```bash
claude mcp list | grep -i playwright
```

### B) Context7 MCP

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

Альтернативный вариант (SSE transport):

```bash
claude mcp add --transport sse context7 https://mcp.context7.com/sse
```

Проверка:

```bash
claude mcp list | grep -i context7
```

### C) Serena MCP (универсальный LSP)

Требуется `uv`:

```bash
brew install uv
```

Установка Serena:

```bash
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context claude-code --project "$(pwd)"
```

Проверка:

```bash
claude mcp list | grep -i serena
```

---

## 9. Superpowers Skills

Superpowers — набор скилов для улучшения работы Claude Code.

Репозиторий: https://github.com/obra/superpowers

### Установка

Запустите интерактивный `claude` и выполните:

```
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### Обновление

```
/plugin update superpowers
```

---

## 10. Проверка окружения

### Чек-лист

| # | Компонент | Команда проверки |
|---|-----------|------------------|
| 1 | VPN | `curl -s https://ifconfig.me` (проверить IP) |
| 2 | Cursor | Должен быть открыт |
| 3 | Python | `python3 --version` |
| 4 | Node.js | `node -v && npm -v` |
| 5 | Homebrew | `brew --version` |
| 6 | Git | `git --version` |
| 7 | GitHub CLI | `gh --version` |
| 8 | Claude CLI | `claude --version` |
| 9 | Авторизация Claude | `claude auth status` |
| 10 | MCP-плагины | `claude mcp list` |
| 11 | Superpowers | Проверить в интерактивном режиме |

### Быстрая проверка всего

Можно попросить Claude проверить окружение, отправив ему этот запрос:

```
Проверь моё окружение для работы. Нужно проверить и вывести статус (установлено / не установлено) для следующих пунктов:

1. VPN подключение
2. Cursor
3. Python
4. Node.js
5. Homebrew
6. Git
7. GitHub CLI (gh)
8. Claude CLI
9. Наличие аккаунта Claude (авторизация)
10. Установлены ли плагины:
    – Context7
    – Playwright
    – Serena (универсальный LSP)
11. Настроенные скилы из ссылки https://github.com/obra/superpowers
```

---

## 11. Автоматическая установка (промпты)

Установка разделена на 2 этапа:
- **Промпт 1** — для Cursor (установка всего до MCP-плагинов)
- **Промпт 2** — для Claude (установка Superpowers + финальная проверка)

---

### Промпт 1: Для Cursor

Скопируйте и отправьте этот промпт в чат Cursor:

```
Ты — установщик dev-окружения. Работаешь в терминале на текущей машине.

Цель: подготовить базовое окружение для Claude Code + MCP:
- VPN (проверка, при необходимости попроси включить вручную)
- Homebrew
- Python
- Node.js
- Git
- GitHub CLI (gh) — ТОЛЬКО установить, НЕ логинить в GitHub
- Claude Code/CLI (claude)
- Авторизация в Claude
- MCP-плагины: Context7, Playwright, Serena

Правила:
1) Сначала сделай диагностику и выведи краткий план (что уже есть / чего нет).
2) Затем выполняй установку/настройку по шагам.
3) Перед изменением конфигов — делай бэкап файлов.
4) Где нужны ручные действия (VPN/логин в браузере) — остановись и попроси меня сделать действие.

ШАГ 0 — Диагностика
- Определи ОС/шелл:
  - uname -a
  - sw_vers (если macOS)
  - echo $SHELL
- Проверь VPN "по факту":
  - ifconfig | egrep "utun|ppp|tun" || true
  - curl -s https://ifconfig.me ; echo
Если VPN не выглядит подключенным — попроси меня включить VPN вручную.

ШАГ 1 — Homebrew
- Если brew нет: установи Homebrew официальным способом и добавь в PATH для zsh.
- Примечание: если запросит sudo — это пароль текущего пользователя macOS.
- Затем: brew update

ШАГ 2 — Базовые пакеты
- brew install python node git gh uv

ШАГ 3 — Claude Code
- npm install -g @anthropic-ai/claude-code
- Проверь: claude --version
- Авторизация: claude auth login
  (откроется браузер; дождись завершения)

ШАГ 4 — MCP-плагины

A) Playwright MCP:
- claude mcp add --scope user playwright -- npx -y @playwright/mcp@latest

B) Context7 MCP:
- claude mcp add context7 -- npx -y @upstash/context7-mcp@latest

C) Serena MCP:
- claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context claude-code --project "$(pwd)"

Проверь все плагины: claude mcp list

ШАГ 5 — Промежуточный отчёт
Выведи статус:
- VPN: подключен / не подключен
- Установлено: brew, python3, node/npm, git, gh, uv, claude
- Авторизация Claude: результат claude auth status
- MCP-плагины: вывод claude mcp list

После этого напиши:
"Базовая установка завершена. Теперь запусти команду `claude` в терминале и отправь ему Промпт 2 для установки Superpowers."
```

---

### Промпт 2: Для Claude

После выполнения Промпта 1 запустите `claude` в терминале и отправьте этот промпт:

```
Установи Superpowers и проверь окружение.

ШАГ 1 — Superpowers Skills
Выполни команды:
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

ШАГ 2 — Финальная проверка окружения
Проверь и выведи статус (установлено / не установлено) для всех пунктов:

1. VPN подключение (curl -s https://ifconfig.me)
2. Python (python3 --version)
3. Node.js (node -v && npm -v)
4. Homebrew (brew --version)
5. Git (git --version)
6. GitHub CLI (gh --version)
7. Claude CLI (claude --version)
8. Авторизация Claude (claude auth status)
9. MCP-плагины (claude mcp list):
   - Context7
   - Playwright
   - Serena
10. Superpowers Skills — установлены?

Выведи итоговую таблицу со статусами.
```

---

## Troubleshooting

### VPN не работает
- Проверьте, что VPN-клиент запущен и подключен
- Проверьте IP: `curl -s https://ifconfig.me`

### Claude не авторизуется
- Убедитесь, что VPN включен
- Попробуйте `/login` в интерактивном режиме claude

### Homebrew не найден после установки
Добавьте в PATH:
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```

### MCP-плагин не работает
Перезапустите Claude Code и проверьте:
```bash
claude mcp list
```
