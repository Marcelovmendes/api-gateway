# PlaySwap API Gateway

API Gateway baseado em KrakenD para o projeto PlaySwap, responsável por rotear requisições para os microserviços Spotify e YouTube.

## Arquitetura

```
┌──────────────┐
│   Frontend   │
│  (porta 3000)│
└──────┬───────┘
       │
       ▼
┌──────────────┐      ┌─────────────────────┐
│  API Gateway │──────│  spotify-service    │
│  (porta 8000)│      │  (porta 8080)       │
└──────┬───────┘      └─────────────────────┘
       │
       │              ┌─────────────────────┐
       └──────────────│  youtube-service    │
                      │  (porta 8081)       │
                      └─────────────────────┘
```

**Obs:** Endpoints de OAuth (login/callback) são chamados diretamente nos backends devido a limitações de redirect na versão Community do KrakenD.

## Estrutura

```
config/
├── krakend.tmpl              # Configuração principal
├── settings/
│   └── settings.tmpl         # Variáveis globais
└── templates/
    ├── endpoints.tmpl        # Orquestrador
    └── *-endpoints.tmpl      # Endpoints por domínio
```

## Requisitos

- Docker
- Docker Compose

## Variáveis de Ambiente

| Variável | Descrição | Default |
|----------|-----------|---------|
| `SPOTIFY_SERVICE_URL` | URL do spotify-service | `http://host.docker.internal:8080` |
| `YOUTUBE_SERVICE_URL` | URL do youtube-service | `http://host.docker.internal:8081` |
| `FRONTEND_URL` | URL do frontend (CORS) | `http://127.0.0.1:3000` |

## Execução

```bash
docker-compose up
```

Gateway disponível em `http://localhost:8000`.

## Configurações

- **Timeout:** 30s
- **CORS:** Habilitado para `FRONTEND_URL`
- **Encoding:** `no-op` (pass-through)

## Tecnologias

- [KrakenD](https://www.krakend.io/) v2.7
- Docker
