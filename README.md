# Docker-контейнеризация и хранение данных

Проект содержит два образа

| Сервис   | Описание       | Образы                    | Размер    | Эндпоинты              |
|----------|----------------|---------------------------|-----------|------------------------|
| frontend | Vue.js + Nginx | momo-store-frontend:1.0.0 | ~22.5 MB  | http://localhost:8081/ |
| backend  | Go             | momo-store-backend:1.0.0  | ~14.75 MB |                        |

## Запуск образов

```bash
docker compose build

docker compose up -d
```

## Проверка доступности сервисов

```bash
curl http://localhost:8081/health

curl http://localhost/
```

## Остановка сервисов

```bash
docker compose down
```

## Проверка размеров образов

```bash
docker images momo-store-backend:1.0.0 momo-store-frontend:1.0.0
```

## Масштабирование сервиса

```bash
docker compose up -d --scale frontend=2 --scale backend=2
```

## Проверка образов

```bash
docker compose build
trivy image --severity HIGH,CRITICAL --ignore-unfixed momo-store-backend:1.0.0
trivy image --severity HIGH,CRITICAL --ignore-unfixed momo-store-frontend:1.0.0
```