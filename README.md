# Local AI Chat

Chat local com [Open WebUI](https://openwebui.com/), [Ollama](https://ollama.com/) e PostgreSQL, executado com Docker Compose.

## Requisitos

- Docker Desktop ou Docker Engine
- Docker Compose V2
- GPU NVIDIA opcional

## Deploy

Clone o repositório e crie o arquivo de ambiente:

```bash
cp .env.example .env
```

No Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Edite `.env` e troque `POSTGRES_PASSWORD` e `WEBUI_SECRET_KEY`. Para gerar uma chave segura:

```bash
openssl rand -hex 32
```

Inicie os serviços:

```bash
docker compose pull
docker compose up -d
```

Acesse [http://localhost:3000](http://localhost:3000). O primeiro usuário cadastrado será o administrador.

No seletor de modelos, informe um modelo Ollama, como `llama3.2:1b`, e confirme o download.

## GPU NVIDIA

Requer drivers NVIDIA e NVIDIA Container Toolkit.

```bash
docker compose -f compose.yaml -f compose.gpu.yaml up -d
```

## Comandos úteis

```bash
# Estado dos serviços
docker compose ps

# Logs
docker compose logs -f

# Reiniciar
docker compose restart

# Parar sem apagar os dados
docker compose down
```

Os dados do PostgreSQL, Open WebUI e modelos Ollama são mantidos em volumes Docker.

> Não exponha esta instalação diretamente na internet sem HTTPS, controle de acesso e revisão de segurança.
