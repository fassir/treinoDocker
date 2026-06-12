<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&height=200&color=gradient&customColorList=0:1F9BD4,50:2E75B6,100:16265F&text=treinoDocker&fontColor=ffffff&fontSize=46&fontAlignY=35&desc=Docker%20na%20Pr%C3%A1tica%20%7C%20Do%20B%C3%A1sico%20ao%20Avan%C3%A7ado&descAlignY=55&descSize=18&animation=twinkling" width="100%" />

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://kernel.org)
[![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org)
[![Alpine](https://img.shields.io/badge/Alpine%20Linux-0D597F?style=for-the-badge&logo=alpinelinux&logoColor=white)](https://alpinelinux.org)

[![Exercícios](https://img.shields.io/badge/exercícios-4%2B-blue?style=flat-square)]()
[![Nível](https://img.shields.io/badge/nível-básico%20ao%20avançado-orange?style=flat-square)]()
[![Multi-stage](https://img.shields.io/badge/técnica-multi--stage%20build-purple?style=flat-square)]()
[![Status](https://img.shields.io/badge/status-ativo-brightgreen?style=flat-square)]()

</div>

---

## 📋 Sobre o Projeto

O **treinoDocker** é uma série de **exercícios práticos progressivos** com Docker, projetados para construir conhecimento de forma gradual — do básico ao avançado. Cada exercício está organizado em sua própria pasta numerada, com um `Dockerfile` dedicado e contexto mínimo para facilitar o aprendizado e a experimentação.

O projeto cobre os conceitos fundamentais do Docker: execução de containers, customização de imagens, volumes, variáveis de ambiente e técnicas avançadas como **multi-stage builds** para otimização de imagens de produção.

---

<div align="center">

## 🛠️ Stack de Tecnologias

[![My Skills](https://skillicons.dev/icons?i=docker,linux,nginx,bash&theme=dark)](https://skillicons.dev)

</div>

| Tecnologia | Finalidade |
|---|---|
| **Docker** | Motor de containers — build, run, push |
| **Alpine Linux** | Imagem base ultra-leve (~5 MB) |
| **Nginx** | Servidor web para servir HTML customizado |
| **Linux / Bash** | Ambiente de execução e scripts |

---

## 📁 Estrutura do Repositório

```
treinoDocker/
│
├── numero01/
│   └── Dockerfile              # Exercício 1: Alpine com mensagem
│
├── numero02/
│   ├── Dockerfile              # Exercício 2: Nginx com HTML via volume
│   └── index.html              # Página HTML customizada
│
├── numero03/
│   └── Dockerfile              # Exercício 3: Variáveis de ambiente
│
├── numero04/
│   └── Dockerfile              # Exercício 4: Multi-stage build
│
└── README.md                   # Este arquivo
```

---

## 🏋️ Exercícios

### 📦 Exercício 1 — Alpine imprimindo mensagem

<details>
<summary>🔍 <strong>Ver detalhes do Exercício 1</strong></summary>

<br>

**Objetivo:** Criar um container Docker minimalista baseado em Alpine Linux que imprime uma mensagem ao ser executado.

**Conceitos abordados:**
- Instrução `FROM` — imagem base
- Instrução `CMD` — comando padrão do container
- Diferença entre `CMD` e `ENTRYPOINT`

```dockerfile
# numero01/Dockerfile
FROM alpine:3.18

LABEL maintainer="Fabio Piassi <github.com/fassir>"
LABEL description="Container minimalista que imprime uma mensagem"

CMD ["echo", "Olá! Este é meu primeiro container Docker com Alpine!"]
```

**Executando:**

```bash
# Build da imagem
docker build -t treino-01-alpine ./numero01

# Executar o container
docker run --rm treino-01-alpine
# Saída: Olá! Este é meu primeiro container Docker com Alpine!

# Inspecionar a imagem (tamanho ~7 MB!)
docker images treino-01-alpine
```

**Por que Alpine?**

| Imagem | Tamanho |
|---|---|
| `ubuntu` | ~77 MB |
| `debian:slim` | ~74 MB |
| **`alpine`** | **~7 MB** |

Alpine é ideal para containers de produção pelo seu tamanho mínimo e superfície de ataque reduzida.

</details>

---

### 🌐 Exercício 2 — Nginx com HTML customizado via volume

<details>
<summary>🔍 <strong>Ver detalhes do Exercício 2</strong></summary>

<br>

**Objetivo:** Executar um servidor Nginx que serve uma página HTML customizada, utilizando volumes Docker para injetar o conteúdo sem reconstruir a imagem.

**Conceitos abordados:**
- Volumes Docker (`-v`)
- Exposição de portas (`EXPOSE`, `-p`)
- Imagem base `nginx`
- Diferença entre copiar arquivos na imagem vs. montar via volume

```dockerfile
# numero02/Dockerfile
FROM nginx:alpine

LABEL maintainer="Fabio Piassi <github.com/fassir>"

EXPOSE 80

# Página padrão (substituível via volume em runtime)
COPY index.html /usr/share/nginx/html/index.html
```

```html
<!-- numero02/index.html -->
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Treino Docker - Nginx</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 50px; background: #1a1a2e; color: #e0e0e0; }
        h1 { color: #2496ED; }
    </style>
</head>
<body>
    <h1>🐳 Docker + Nginx Funcionando!</h1>
    <p>Página customizada servida pelo Nginx em container Docker.</p>
    <p>Exercício 2 — treinoDocker</p>
</body>
</html>
```

**Executando:**

```bash
# Build da imagem
docker build -t treino-02-nginx ./numero02

# Opção A: usando COPY do Dockerfile (arquivo já embutido)
docker run -d -p 8080:80 --name nginx-treino treino-02-nginx

# Opção B: sobrescrevendo via volume (sem rebuild)
docker run -d -p 8080:80 \
  -v $(pwd)/numero02/index.html:/usr/share/nginx/html/index.html \
  --name nginx-volume \
  nginx:alpine

# Acessar no browser
open http://localhost:8080

# Parar e remover
docker stop nginx-treino && docker rm nginx-treino
```

</details>

---

### 🔧 Exercício 3 — Container com variáveis de ambiente

<details>
<summary>🔍 <strong>Ver detalhes do Exercício 3</strong></summary>

<br>

**Objetivo:** Demonstrar o uso de variáveis de ambiente para configurar o comportamento do container sem modificar a imagem.

**Conceitos abordados:**
- Instrução `ENV` — variáveis de ambiente padrão
- Flag `-e` do `docker run` — sobrescrever variáveis em runtime
- Arquivo `.env` com `--env-file`
- Uso de `ARG` vs `ENV`

```dockerfile
# numero03/Dockerfile
FROM alpine:3.18

LABEL maintainer="Fabio Piassi <github.com/fassir>"

# Variáveis de ambiente com valores padrão
ENV APP_NAME="treinoDocker"
ENV APP_ENV="development"
ENV APP_PORT="8000"
ENV MENSAGEM="Olá do container com variáveis de ambiente!"

CMD sh -c 'echo "App: $APP_NAME | Ambiente: $APP_ENV | Porta: $APP_PORT"; echo "$MENSAGEM"'
```

**Executando:**

```bash
# Build
docker build -t treino-03-env ./numero03

# Com variáveis padrão
docker run --rm treino-03-env
# App: treinoDocker | Ambiente: development | Porta: 8000

# Sobrescrevendo variáveis em runtime
docker run --rm \
  -e APP_ENV="production" \
  -e APP_PORT="443" \
  -e MENSAGEM="Rodando em produção!" \
  treino-03-env

# Usando arquivo .env
echo "APP_ENV=staging\nAPP_PORT=8080" > .env
docker run --rm --env-file .env treino-03-env

# Inspecionar variáveis de um container em execução
docker inspect treino-03-env | grep -A 10 '"Env"'
```

</details>

---

### 🏗️ Exercício 4 — Multi-stage build

<details>
<summary>🔍 <strong>Ver detalhes do Exercício 4</strong></summary>

<br>

**Objetivo:** Utilizar multi-stage builds para criar imagens de produção otimizadas — separando o estágio de build (com todas as ferramentas) do estágio final (apenas o binário/artefato).

**Conceitos abordados:**
- Instrução `FROM ... AS nome-do-stage`
- `COPY --from=stage-anterior`
- Redução drástica do tamanho final da imagem
- Segurança: imagem de produção sem ferramentas de desenvolvimento

```dockerfile
# numero04/Dockerfile
# ===== STAGE 1: Build =====
FROM golang:1.21-alpine AS builder

WORKDIR /app

# Copiar e compilar o código Go
COPY main.go .
RUN go build -o hello-app main.go

# ===== STAGE 2: Produção (imagem final) =====
FROM alpine:3.18

LABEL maintainer="Fabio Piassi <github.com/fassir>"
LABEL description="Multi-stage build: apenas o binário compilado"

WORKDIR /app

# Copiar APENAS o binário compilado do stage anterior
COPY --from=builder /app/hello-app .

EXPOSE 8080

CMD ["./hello-app"]
```

**Executando:**

```bash
# Build com multi-stage
docker build -t treino-04-multistage ./numero04

# Comparar tamanhos
docker images | grep treino-04

# RESULTADO:
# treino-04-multistage     latest    ~12 MB   ← apenas binário
# golang:1.21-alpine       latest    ~250 MB  ← ferramentas de build
```

**Diagrama — Antes vs. Depois:**

```
SEM multi-stage:                COM multi-stage:
┌───────────────────┐           ┌───────────────┐
│  golang:alpine    │           │  golang:alpine│ ← builder (descartado)
│  + código-fonte   │           │  + compilação │
│  + dependências   │           └───────┬───────┘
│  + compilador     │                   │ COPY --from=builder
│  + ferramentas    │                   ▼
│  = ~250 MB        │           ┌───────────────┐
└───────────────────┘           │  alpine:3.18  │
                                │  + binário ✓  │
                                │  = ~12 MB     │
                                └───────────────┘
```

</details>

---

## 📚 Referência Rápida — Comandos Docker

```bash
# ===== IMAGENS =====
docker images                          # Listar imagens locais
docker pull alpine:3.18                # Baixar imagem do registry
docker build -t nome:tag .             # Build de uma imagem
docker rmi nome:tag                    # Remover imagem
docker image prune                     # Remover imagens não utilizadas

# ===== CONTAINERS =====
docker run -d -p 8080:80 nginx         # Executar em background
docker run --rm -it alpine sh          # Executar interativamente (e remover ao sair)
docker ps                              # Listar containers em execução
docker ps -a                           # Listar todos os containers
docker stop <id>                       # Parar container
docker rm <id>                         # Remover container
docker logs <id>                       # Ver logs do container
docker exec -it <id> sh                # Acessar shell do container

# ===== VOLUMES =====
docker volume create meu-volume        # Criar volume nomeado
docker run -v meu-volume:/data nginx   # Montar volume
docker run -v $(pwd)/html:/usr/share/nginx/html nginx  # Bind mount

# ===== LIMPEZA =====
docker system prune -af                # Limpar tudo (containers, imagens, volumes)
```

---

## 📈 Progressão de Aprendizado

| Exercício | Pasta | Conceito Principal | Dificuldade |
|---|---|---|---|
| Alpine básico | `numero01/` | FROM, CMD, imagem base | ⭐ |
| Nginx + volume | `numero02/` | EXPOSE, COPY, volumes | ⭐⭐ |
| Variáveis ENV | `numero03/` | ENV, runtime config | ⭐⭐ |
| Multi-stage build | `numero04/` | Otimização, separação build/prod | ⭐⭐⭐ |

---

## 👤 Autor

<div align="center">

| | |
|---|---|
| **Nome** | Fabio Piassi |
| **LinkedIn** | [linkedin.com/in/fabio-piassi](https://linkedin.com/in/fabio-piassi) |
| **GitHub** | [github.com/fassir](https://github.com/fassir) |

</div>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&section=footer&height=120&color=gradient&customColorList=0:16265F,50:2E75B6,100:1F9BD4" width="100%" />

*Série de exercícios práticos de Docker — do básico ao multi-stage build*

</div>
