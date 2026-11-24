## Шаблон для третьего ДЗ по ИПР

Условие дз доступно по [ссылке](https://github.com/Nikfilk2030/missing_semester_hse_cs/blob/main/2026/amcs/year_1/hw_3/README.md)

## Запуск с Redis

Из корня репозитория:

```bash
cp .env.example .env
docker compose up -d --build
docker compose ps
```

Дождитесь статуса `healthy` у `app` и `redis`. По умолчанию приложение
доступно по адресу http://127.0.0.1:8080/. В `.env` можно изменить
`APP_PORT` (порт на компьютере) и `GREETING` (текст приветствия).
Для применения изменений - `docker compose up -d`.

Прочитать и увеличить счётчик при стандартном порте:

```bash
curl http://127.0.0.1:8080/
curl -X POST http://127.0.0.1:8080/increment
```

Для остановки с сохранением данных - `docker compose down`.

## Запуск с SQLite

Сначала остановите вариант с Redis. Для запуска с SQLite -
`docker compose -f compose.sqlite.yaml up -d --build`.
Для остановки - `docker compose -f compose.sqlite.yaml down`.

## Результаты проверок

- Один контейнер: GET, POST и повторный GET вернули значения 0, 1 и 1.
- SQLite: после пересоздания контейнера счётчик сохранил значение 2.
- Redis: после пересоздания обоих контейнеров счётчик сохранил значение 3.
- При остановленном Redis `GET /healthz` вернул 200, а `GET /readyz` и `POST /increment` вернули 503.
- После запуска Redis без перезапуска приложения `GET /readyz` вернул 200, `GET /` вернул 3, а следующий `POST /increment` вернул 4.

[Команды и результаты проверок](checks.txt) 
[Логи отдельного контейнера](level4.log)
