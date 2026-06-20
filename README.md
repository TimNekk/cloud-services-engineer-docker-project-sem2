## Prod

```bash
docker compose up --build
```

- фронт: `http://localhost`
- API: `http://localhost:8081`

Если нужен scale

```bash
docker compose up --build --scale backend=3
```

## Dev

```bash
docker compose --profile dev up --build
```

- фронт: `http://localhost:8080`
- API: `http://localhost:8081`

