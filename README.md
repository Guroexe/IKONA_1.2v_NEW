# IKONA Telegram Bot

## Локальный запуск

1. Python **3.12** (см. `runtime.txt`).
2. `pip install -r requirements.txt`
3. Скопируйте `.env.example` → `.env`, заполните переменные.
4. Положите `credentials.json` (сервисный аккаунт Google с доступом к таблице) в корень проекта. На Railway достаточно файла в репозитории; `GOOGLE_CREDENTIALS_JSON` в Variables — только запасной вариант, если файла нет.
5. Папка `gifs/new/` с GIF из кода (`privet_1.gif` и др.) — без неё бот отправит текст вместо анимации.
6. Запуск: `python main.py`

## Google Таблица (чтобы аренда и запись работали)

1. В [Google Cloud Console](https://console.cloud.google.com/) у сервисного аккаунта создайте **новый JSON-ключ** (Keys → Add key → JSON). Старые ключи после смены не работают — в логах будет `Invalid JWT Signature`.
2. В Google Sheets → **Доступ** → email из JSON (`client_email`, вид `...@....iam.gserviceaccount.com`) → **Редактор**.
3. `GOOGLE_SHEET_ID` — id из URL таблицы (длинная строка между `/d/` и `/edit`).
4. **Локально:** `credentials.json` рядом с `main.py` и заполненный `.env`. Если Python без `python-dotenv`, бот всё равно читает `.env` сам.
5. **Railway / облако:** положите `credentials.json` в репозиторий (как на старом боте) и redeploy. Переменная `GOOGLE_CREDENTIALS_JSON` нужна только если файла в образе нет.
6. После старта в логах: `source=GOOGLE_CREDENTIALS_JSON` или путь к файлу, `Successfully connected to Google Sheets`, `sheet_id_len` больше 0. Иначе аренда не откроет лист «Май 2026» и т.п.

## Railway

1. Подключите этот репозиторий в Railway.
2. **Start command:** `python main.py`
3. **Variables** (минимум):

| Переменная | Описание |
|------------|----------|
| `TELEGRAM_BOT_TOKEN` | токен от @BotFather |
| `ADMIN_CHAT_ID` | id чата для уведомлений (целое число, может быть отрицательным для групп) |
| `GOOGLE_SHEET_ID` | id Google Spreadsheet |
| `GOOGLE_CREDENTIALS_JSON` | опционально, если нет `credentials.json` в репозитории |
| `POLZA_IKONA_CHAT_API_KEY` | ключ Polza для IKONA AI |

4. Рекомендуется **Volume** на корень приложения, чтобы сохранялись `bot_persistence.pickle` и `casino_win_feed.json`.
5. Один деплой на токен: второй процесс с тем же токеном даёт ошибку `409 Conflict`.

## Безопасность

Секреты не хранятся в коде: задаются через переменные окружения или `.env` (только локально). Файлы `.env` и `credentials.json` в git не коммитьте. После публикации репозитория смените токены и ключи, которые раньше попадали в логи или старые коммиты.
