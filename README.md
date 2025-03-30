# Projeto de Microserviços 

Este projeto é uma aplicação completa baseada em microserviços que demonstra o uso de diversas tecnologias modernas para construção de sistemas escaláveis, portáteis e de fácil manutenção. Ele integra autenticação, operações CRUD, interface de usuário e documentação de APIs, tudo orquestrado via Docker Compose.

## Índice

- [Visão Geral](#visão-geral)
- [Arquitetura do Sistema](#arquitetura-do-sistema)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Como Executar o Projeto](#como-executar-o-projeto)
- [Endpoints e Documentação](#endpoints-e-documentação)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Instruções para Desenvolvimento](#instruções-para-desenvolvimento)
- [Logs e Monitoramento](#logs-e-monitoramento)
- [Licença](#licença)

## Visão Geral

Este projeto foi desenvolvido para servir como portfólio e base para entrevistas, demonstrando conhecimentos em:
- Arquitetura de microserviços
- Containerização com Docker e Docker Compose
- Desenvolvimento backend com Node.js/Express e frontend com React
- Integração com bancos de dados (MongoDB), cache (Redis) e mensageria (RabbitMQ)
- Documentação de APIs com Swagger

## Arquitetura do Sistema

O sistema é composto por vários serviços independentes que se comunicam entre si:

- **Auth Service**: Responsável pelo cadastro e autenticação de usuários usando JWT. Publica eventos via RabbitMQ.
- **CRUD Service**: Gerencia operações CRUD para produtos, utilizando MongoDB (via Mongoose) para persistência e Redis para cache.
- **Frontend**: Aplicação React que consome as APIs dos microserviços e fornece uma interface de usuário.
- **API Docs**: Serviço que centraliza a documentação das APIs, oferecendo endpoints separados para o Auth Service e o CRUD Service.
- **Infraestrutura**: Serviços complementares orquestrados via Docker Compose:
  - **MongoDB** para armazenamento de dados.
  - **Redis** para cache.
  - **RabbitMQ** para comunicação assíncrona entre serviços.

![Arquitetura do Sistema](https://via.placeholder.com/960x400?text=Arquitetura+do+Sistema)  
*Observação: Substitua o link acima por um diagrama real se desejar.*

## Tecnologias Utilizadas

- **Backend**:
  - Node.js, Express
  - JSON Web Token (JWT) e bcrypt
  - Mongoose (MongoDB)
  - Redis (cache)
  - RabbitMQ (mensageria com amqplib)
  - Winston e Morgan (logging)
- **Frontend**:
  - React (criado com Create React App)
- **Documentação**:
  - Swagger UI Express, swagger-jsdoc
- **Containerização**:
  - Docker, Docker Compose

## Estrutura do Projeto
```bash
A estrutura do repositório é organizada como um monorepo com microserviços:
microservicos-projeto/ 
├── auth-service/ # Serviço de autenticação │  
├── src/ │ 
│ ├── controllers/ 
│ │ ├── services/ 
│ │ ├── routes/ 
│ │ ├── utils/ 
│ │ └── middlewares/ 
│ ├── Dockerfile 
│ ├── package.json 
│ └── .env 
├── crud-service/ # Serviço CRUD para produtos 
│ ├── src/ 
│ │ ├── controllers/ 
│ │ ├── services/ 
│ │ ├── repositories/ 
│ │ ├── models/ 
│ │ └── routes/ 
│ ├── config/ # (Opcional) Configurações do banco de dados 
│ ├── Dockerfile 
│ ├── package.json 
│ └── .env 
├── frontend/ # Aplicação React para interface do usuário 
│ ├── public/ 
│ ├── src/ 
│ ├── Dockerfile 
│ ├── package.json 
│ └── .env (se necessário) 
├── api-docs/ # Serviço de documentação centralizada com Swagger 
│ ├── swagger/ 
│ │ ├── auth.json # Especificação do Auth Service 
│ │ ├── crud.json # Especificação do CRUD Service 
│ │ └── dummy.json # (Opcional) Exemplo dummy para testes 
│ ├── app.js 
│ ├── Dockerfile 
│ └── package.json 
└── infrastructure/ # Arquivo docker-compose.yml para orquestração dos containers 
  └── docker-compose.yml
  ```
## Como Executar o Projeto

### Pré-requisitos

- Docker e Docker Compose instalados e em execução.
- (Opcional) Git para clonar o repositório.

### Instruções

1. **Clone o repositório** (ou navegue até a pasta do projeto):
   ```bash
   git clone <URL_DO_REPOSITORIO>
   cd microservicos-projeto
2. **Navegue até a pasta de infraestrutura** (ou onde está o docker-compose.yml):
  ```bash
  cd infrastructure
```
3. **Construa e inicie os containers:**
```bash
  docker compose up --build
```
Esse comando irá:
- Construir as imagens para todos os serviços (auth-service, crud-service, frontend, api-docs).
- Iniciar os containers para MongoDB, Redis e RabbitMQ.

4. **Acesse os serviços:**

- Auth Service: http://localhost:3000

- CRUD Service: http://localhost:3001

- Frontend: http://localhost:3002

- Documentação da API: http://localhost:3003/api-docs
(A partir daí, você pode acessar as documentações específicas em /api-docs/auth e /api-docs/crud)

- RabbitMQ Management: http://localhost:15672 (usuário/senha padrão: guest/guest)

5. **Para parar os containers: No terminal onde os containers estão rodando, pressione Ctrl + C e depois execute:**
```bash
docker compose down
```
### Endpoints e Documentação

- Documentação Centralizada:
  - http://localhost:3003/api-docs
    Uma página index com links para:
  - Auth Service API: http://localhost:3003/api-docs/auth
  - CRUD Service API: http://localhost:3003/api-docs/crud
- Auth Service Endpoints (exemplos):
  - POST /auth/register – Registra um novo usuário.
  - POST /auth/login – Autentica o usuário e retorna um token JWT.
- CRUD Service Endpoints (exemplos):
  - GET /products – Lista todos os produtos (utilizando cache Redis).
  - POST /products – Cria um novo produto.
  - GET /products/{id} – Retorna um produto específico.
  - PUT /products/{id} – Atualiza um produto.
  - DELETE /products/{id} – Exclui um produto.
    
### Variáveis de Ambiente
Cada serviço possui um arquivo .env para configurar variáveis importantes. Exemplos:

### Auth Service (.env)
```bash
PORT=3000
JWT_SECRET=seuSegredo
RABBITMQ_URL=amqp://rabbitmq
```
### CRUD Service (.env)
```bash
PORT=3001
MONGO_URI=mongodb://mongo:27017/cruddb
RABBITMQ_URL=amqp://rabbitmq
REDIS_URL=redis://redis:6379
```
####Frontend
Pode ser configurado conforme necessário.

### Instruções para Desenvolvimento
Desenvolvimento Local:
- CRUD Service Endpoints (exemplos):
  - Cada serviço pode ser executado individualmente em modo de desenvolvimento utilizando npm run dev (com nodemon) para reinicialização automática.
- Testes e Debug:
  - Utilize ferramentas como Postman ou Insomnia para testar os endpoints.
  - Confira os logs de cada serviço usando:
```bash
docker compose logs <nome-do-serviço>
```
- Documentação:
  - Acesse os endpoints de documentação para verificar a interface Swagger.
Testes e Debug:

### Logs e Monitoramento
- Loggin:
  Os serviços utilizam Morgan para log de requisições HTTP e Winston para logs estruturados.
- Monitoramento:
  - RabbitMQ possui uma interface de gerenciamento em http://localhost:15672.
  - MongoDB e Redis podem ser monitorados via suas respectivas ferramentas e logs.
    
Licença
Este projeto é licenciado sob a MIT License.
