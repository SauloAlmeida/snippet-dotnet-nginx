# Dotnet Nginx (Snippet project)

## Motivação
Desenvolver uma implementação capaz de executar várias instâncias simultaneamente, realizar balanceamento de carga e que seja simples.

## Como funciona
Ao iniciar a aplicação com o Docker Compose, o nginx entrará em ação para executar o proxy reverso, permitindo assim a realização do balanceamento de carga através da criação de múltiplas instâncias utilizando o Docker Compose.

### Código
```yml
version: '3.4'

services:
  api:
    image: sp-dotnet-nginx
    build: ./src/API/
    ports:
      - "5000" # Não será exposto para internet, mas o nginx irá realizar o proxy reverso através do network.
  app: 
    image: nginx:alpine
    volumes:
       - ./src/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
       - api
    ports:
       - "4000:4000"
```

A partir da configuração abaixo que será feito o proxy reverso, que o APP ouvirá a porta 5000 da API.

```text
user nginx;

events {
    worker_connections 1000;
}

http {
  server {
    listen 4000;
    location / {
      proxy_pass http://api:5000;
    }
  }
}
```

<hr>

### Instalação

1. Clone o projeto

```bash
git clone https://github.com/SauloAlmeida/snippet-dotnet-nginx
```

2. Acesse o diretório
```bash
cd snippet-dotnet-nginx
```

## Rodar o projeto

1. Rode o projeto
```bash
docker compose up --scale api=5
```

2. Acesse a rota 
```bash
http://localhost:4000/weatherforecast
```