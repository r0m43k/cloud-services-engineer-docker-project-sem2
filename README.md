frontend Dockerfile- первая стадия сборки Vue на образе node.js alpine
frontend Dockerfile- вторая стадия запуска runtime. Порт: 80

backend Dockerfile- первая стадия собирает Go-приложение на образе golang alpine
backend Dockerfile- вторая стадия запускает файл на образе alpine на порту 8081

Запуск:
```bash
docker compose build
docker compose up -d