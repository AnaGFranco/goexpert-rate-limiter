# Desafio Go: Rate Limiter

O Rate Limiter é uma funcionalidade que limita o número de requisições que podem ser feitas a um serviço em um determinado período de tempo, ajudando a controlar o tráfego e prevenir sobrecargas no servidor.

## Funcionamento

O rate limiter implementado neste projeto utiliza Redis para armazenar e controlar as requisições. Ele suporta limitação baseada em IP e também em tokens de acesso únicos. Como funciona:

- **IP Rate Limiting**: Limita o número de requisições por segundo de um mesmo endereço IP.
- **Token Rate Limiting**: Permite configurar limites específicos para diferentes tokens de acesso, cada um com seu próprio tempo de expiração e contagem de requisições.

## Configuração

### Pré-requisitos

Verifique se o Docker está instalado e configurado no seu ambiente.

### Configuração no Docker Compose

```yaml
version: '3.8'

services:
  redis:
    image: "redis:alpine"
    ports:
      - "6379:6379"
    networks:
      - app-network
      
  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - redis
    environment:
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - REDIS_PASSWORD=
    networks:
      - app-network
    # App:
    #   - A aplicação estará disponível em http://localhost:8080.
    #   - Utilize o cabeçalho API_KEY para autenticar as requisições.

networks:
  app-network:
    driver: bridge
   
```

### Execução

Para iniciar o projeto:

1. Execute `docker-compose up --build` na raiz do projeto.
2. A aplicação estará disponível em `http://localhost:8080`.

### Uso

- **API_KEY**: Para autenticar as requisições, adicione um cabeçalho `API_KEY` com o token correto. Exemplo: `curl -H "API_KEY: token1" http://localhost:8080/`.

### Teste

- Após iniciar o servidor, teste o rate limiter utilizando ferramentas como cURL para verificar se as requisições estão sendo limitadas conforme configurado.

---
