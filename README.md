## Prod

```bash
docker compose up --build
```

- фронт: `http://localhost`
- API: `http://localhost:8081`

Если нужен scale:

```bash
docker compose up --build --scale backend=3
```

## Dev

```bash
docker compose --profile dev up --build
```

- фронт: `http://localhost:8080`
- API: `http://localhost:8081`

## Образы

Оба образа через multi-stage, чтобы финальный образ был меньше и без лишних dev-зависимостей.
Размеры у меня получились примерно такие: backend `23 MB`, frontend `60 MB`.

## Trivy

Для проверки уязвимостей можно прогнать `trivy image` и `trivy config .`  
Он уже подключен в CI/CD
