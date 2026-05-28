#frontend Dockerfile- первая стадия сборки Vue на образе node.js alpine
#frontend Dockerfile- вторая стадия запуска runtime. Порт: 80

#backend Dockerfile- первая стадия собирает Go-приложение на образе golang alpine
#backend Dockerfile- вторая стадия запускает файл на образе alpine на порту 8081

Запуск
```bash
docker compose build
docker compose up -d
```

ПРоверка
```bash
docker compose ps
curl -I http://localhost
curl -i http://localhost:8081/health
```

Проверка пользователей
```bash
docker compose exec backend whoami
docker compose exec frontend whoami
docker compose exec backend-balancer whoami
```

Масштабирование backend
```bash
docker compose up -d --scale backend=3
docker compose ps
curl -i http://localhost:8081/health
```

Оставить один экземпляр backend:
```bash
docker compose up -d --scale backend=1
```

Сканирование Trivy
```bash
trivy image backend:latest
trivy image frontend:latest
```

Текущие размеры:
- `backend:latest` — 21.2MB
- `frontend:latest` — 50MB

backend использует multi-stage build. На стадии `builder` применяется `golang:1.19-alpine`. Зависимости скачиваются отдельно через `go mod download`, что улучшает кэширование. Приложение собирается в один бинарный файл `server`. В финальный образ `alpine:3.20` копируется только готовый бинарник. Go SDK, исходники и инструменты сборки не попадают в runtime-образ.


Frontend собирается в Node.js builder-стадии, а в финальный образ попадают только статические файлы из `dist`. Поэтому итоговый размер `frontend:latest` — около `50MB`.

Проверить:
```bash
docker images backend
docker images frontend
```

Логи
```bash
docker compose logs backend
docker compose logs frontend
docker compose logs backend-balancer
```