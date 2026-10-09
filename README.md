# Деплой веб-приложений на облачный сервер

Ручной деплой двух проектов — Kittygram (Django REST Framework + React) и Taski — на виртуальную машину в Яндекс Облаке без Docker.

## Что сделано

- Бэкенд работает через Gunicorn как служба systemd (`infra/gunicorn_kittygram.service`) и сам поднимается после перезагрузки сервера.
- Nginx раздаёт статику фронтенда и медиафайлы, а запросы к `/api/` и `/admin/` проксирует на Gunicorn. Два проекта работают на одном сервере под разными доменами.
- HTTPS: SSL-сертификаты Let's Encrypt, выпущенные через Certbot.

## Технологии

Python 3.9, Django 3.2, Django REST Framework, Gunicorn, Nginx, systemd, Certbot, Linux (Ubuntu), React.

## Структура

- `backend/` — API Kittygram
- `frontend/` — React-приложение
- `infra/` — конфигурация Nginx и systemd-юнит для Gunicorn

## Как развернуть

1. Клонировать репозиторий на сервер, в `backend/` создать виртуальное окружение, установить зависимости, выполнить `migrate` и `collectstatic`.
2. Собрать фронтенд (`npm i && npm run build`) и скопировать сборку в `/var/www/kittygram/`.
3. Скопировать `infra/gunicorn_kittygram.service` в `/etc/systemd/system/` и запустить службу: `sudo systemctl enable --now gunicorn_kittygram`.
4. Подключить конфигурацию Nginx из `infra/default`, проверить её командой `sudo nginx -t` и перезапустить Nginx.
5. Выпустить сертификат: `sudo certbot --nginx`.

## Что я вынес из проекта

- Как устроен продакшен-стек без контейнеров: WSGI-сервер, обратный прокси, системные службы.
- Работа с Linux-сервером по SSH и настройка HTTPS.

## Автор

Иван Богатов — [GitHub](https://github.com/Vantied) · Telegram [@Ivan_bogatov55](https://t.me/Ivan_bogatov55)
