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

Edite `.env` e troque `POSTGRES_PASSWORD`, `WEBUI_SECRET_KEY` e
`OPEN_TERMINAL_API_KEY`. Gere chaves diferentes para o Open WebUI e para o
Open Terminal. Para gerar uma chave segura:

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

## Open Terminal

O Open Terminal fornece ao modelo um ambiente isolado para ler, criar e editar
arquivos. A pasta local `workspace/` e o diretório `/home/user` do
contêiner representam o mesmo conteúdo.

Depois de iniciar os serviços, conecte o Open Terminal no Open WebUI:

1. Acesse **Configurações > Admin > Integrações**.
2. Na seção **Open Terminal**, adicione uma conexão.
3. Use a URL `http://open-terminal:8000`.
4. Informe a mesma `OPEN_TERMINAL_API_KEY` configurada no arquivo `.env`.
5. Mantenha o tipo de autenticação como **Bearer**.
6. Selecione **Filesystem** em **Chat Uploads** e salve.

Em seguida, selecione o terminal pelo botão de terminal/nuvem na área de chat.
O modelo utilizado deve estar configurado com **Function Calling: Native**.

O Open Terminal não expõe portas para o host e não recebe acesso ao restante
do projeto, ao arquivo `.env` ou ao socket do Docker. Somente `workspace/` é
montada com permissão de leitura e escrita.

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
