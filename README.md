# Docker-контейнеризация и хранение данных

Проект содержит два образа

| Сервис   | Описание       | Образы                    | Размер   | Эндпоинты                    |
|----------|----------------|---------------------------|----------|------------------------------|
| frontend | Vue.js + Nginx | momo-store-frontend:1.0.0 | ~95.6 MB | http://127.0.0.1/momo-store/ |
| backend  | Go             | momo-store-backend:1.0.0  | ~51.8 MB |                              |

## Запуск образов

для dev среды

```bash
docker compose build

docker compose up -d
```

для production подтягивает образы с
DockerHub [frontend](https://hub.docker.com/repository/docker/orbitiumnos/docker-project-frontend)
и [backend](https://hub.docker.com/repository/docker/orbitiumnos/docker-project-backend) если верно заполнен .env

```bash
docker compose -f docker-compose.prod.yml up -d
```

## Проверка доступности сервисов

```bash
curl http://127.0.0.1:8080/momo-store/

curl http://127.0.0.1:8081/health
```

## Остановка сервисов

```bash
docker compose down
```

## Проверка размеров образов

```bash
docker images momo-store-backend:1.0.0 

docker images momo-store-frontend:1.0.0
```

## Масштабирование сервиса

```bash
docker compose up -d --scale frontend=2 --scale backend=2
```

## Проверка образов на уязвимости

```bash
docker compose build
trivy image --severity HIGH,CRITICAL --ignore-unfixed momo-store-backend:1.0.0
trivy image --severity HIGH,CRITICAL --ignore-unfixed momo-store-frontend:1.0.0
```