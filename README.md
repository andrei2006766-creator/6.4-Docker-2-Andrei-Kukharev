# 6.4. Docker. Часть 2 — Андрей Кухарев

## Задание 1. Docker Compose — зачем он нужен

Docker Compose — это инструмент, который описывает весь сервис в одном
файле: какие контейнеры запускать, какие у них порты, тома и сети.
Вместо десятка команд docker run достаточно одной — docker compose up -d.
Удобно тем, что конфигурация лежит в git целиком, её видно и можно
повторить на другой машине, не вспоминая, что я вводил в прошлый раз.

## Задание 2. Первичные настройки
Текст конфига: 

```yaml
version: '3'

services:

volumes:

networks:
  kukharev-a-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
```
## Задание 3. Prometheus

Добавлен сервис Prometheus, контейнер `kukharev-a-netology-prometheus`,
порт 9090, конфигурация из папки `prometheus/`, данные в томе `prometheus_data`.

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.36.2
    container_name: kukharev-a-netology-prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/:/etc/prometheus/
      - prometheus_data:/prometheus
    networks:
      - kukharev-a-my-netology-hw

volumes:
  prometheus_data: {}

networks:
  kukharev-a-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
```