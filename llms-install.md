# Установка ozon-mcp-ru агентом

Документ для ИИ-агента, который ставит сервер за человека. Человеку удобнее
[README](README.md).

## 1. Проверить uv

```bash
uvx --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 2. Прописать сервер

Claude Desktop: `~/Library/Application Support/Claude/claude_desktop_config.json`
(macOS) или `%APPDATA%\Claude\claude_desktop_config.json` (Windows).
Cline: `cline_mcp_settings.json`.

```json
{
  "mcpServers": {
    "ozon": {
      "command": "uvx",
      "args": ["ozon-mcp-ru"],
      "env": {
        "OZON_CLIENT_ID": "<значение>",
        "OZON_API_KEY": "<значение>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## 3. Ключи

| переменная | тип | где взять |
|---|---|---|
| `OZON_CLIENT_ID` | секрет | Client-Id из кабинета seller.ozon.ru, Настройки → API-ключи. |
| `OZON_API_KEY` | секрет | Api-Key из той же пары. Оба уходят в одноимённые заголовки. |

Значения спрашиваются у человека и в репозиторий не пишутся. Второй путь, без
переменных окружения: запустить сервер и вызвать `ozon_add_cabinet`,
ключи лягут в `~/.marketplace-mcp/cabinets.json` с правами 600.

## 4. Проверить

Перезапустить клиент и вызвать `ozon_check_auth`. Ответ «ключей нет»
означает, что сервер поднялся, а ключи не дошли: смотреть шаг 3. Каталог
отвечает `ozon_list_sections`, в нём 441 методов.

## Если не поднимается

- `uvx` не найден: шаг 1, потом перезапустить клиент, он читает PATH при старте.
- Пусто в списке инструментов: клиент не перечитал конфигурацию, нужен рестарт.
- Ошибка авторизации при верных ключах: активный кабинет в
  `~/.marketplace-mcp/cabinets.json` имеет приоритет над переменными окружения.
