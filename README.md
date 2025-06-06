# RSS Keywords Tracker

Веб-сервис на FastAPI для отслеживания появления ключевых слов в новостях 
из RSS лент.
## Технологии
- Python
- FastAPI
- SQLAlchemy
- SQLite
## Функциональность

Сервис состоит из двух основных частей:
1. **Модуль сбора данных с RSS лент** - автоматически сканирует настроенные RSS источники и сохраняет новости, содержащие ключевые слова.
2. **Веб-сервис для управления и просмотра** - предоставляет API для:
   - Управления RSS источниками (добавление, просмотр, удаление)
   - Управления ключевыми словами (добавление, просмотр, удаление)
   - Просмотра найденных новостей с фильтрацией по ключевым словам

## Структура проекта

```
.
├── app/
│   ├── __init__.py            # Инициализация пакета
│   ├── models.py              # Модели SQLAlchemy и Pydantic
│   ├── database.py            # Настройки подключения к базе данных
│   ├── scanner.py             # Модуль сканирования RSS лент
│   ├── crud.py                # CRUD-операции с базой данных
│   └── api.py                 # API роуты для FastAPI
├── config.py                  # Настройки конфигурации
├── main.py                    # Основной файл запуска приложения
├── requirements.txt           # Зависимости проекта
└── README.md                  # Документация проекта

```

## Требования

- Python 3.11 или выше
- Зависимости из файла requirements.txt

## Установка и запуск

1. **Клонировать репозиторий:**
```bash
git clone https://github.com/kkedasnu/py_miigaik.git
cd py_miigaik
```

2. **Создать и активировать виртуальное окружение:**
```bash
python3 -m venv venv
source venv/bin/activate  # На Windows: venv\Scripts\activate
```

3. **Установить зависимости:**
```bash
pip install -r requirements.txt
```
4. **Выставить интервал опроса источников**
В файле `config.py` установить параметр `SCAN_INTERVAL_SECONDS` в секундах 
   (по умолчанию 60 секунд)
4. **Запустить сервис:**
```bash
python main.py
```

Сервис будет доступен по адресу: http://localhost:8000

## API Endpoints

- `GET /docs` - Документация Swagger UI для API

### RSS Источники
- `GET /sources` - Получить список всех RSS источников
- `POST /sources` - Добавить новый RSS источник
- `GET /sources/{source_id}` - Получить информацию о конкретном RSS источнике
- `DELETE /sources/{source_id}` - Удалить RSS источник

### Ключевые слова
- `GET /keywords` - Получить список всех ключевых слов
- `POST /keywords` - Добавить новое ключевое слово
- `GET /keywords/{keyword_id}` - Получить информацию о конкретном ключевом слове
- `DELETE /keywords/{keyword_id}` - Удалить ключевое слово

### Новости
- `GET /news` - Получить список всех найденных новостей
- `GET /news?keyword_id={keyword_id}` - Получить новости, содержащие конкретное ключевое слово

## Как это работает

1. При запуске сервиса автоматически создается база данных SQLite (`rss_tracker.db`) с необходимыми таблицами, если она не существует.
2. Сканер RSS лент запускается в отдельном потоке и периодически (каждую 1 минуту) проверяет настроенные источники.
3. Для каждой новой новости проверяется наличие настроенных ключевых слов в заголовке и содержимом.
4. Новости, содержащие хотя бы одно ключевое слово, сохраняются в базу данных с указанием какие именно ключевые слова были найдены.
5. Через веб-интерфейс API можно управлять источниками, ключевыми словами и просматривать найденные новости.
6. Все действия сканера логируются в файл `rss-tracker.log`.

