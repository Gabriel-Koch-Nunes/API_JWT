
# 🔐 API de Autenticação e Autorização com JWT

API RESTful construída com **Spring Boot 3**, **JWT**, **Spring Security**, **Swagger**, **Prometheus/Grafana** e **Docker**. Permite o registro e autenticação de usuários com controle de acesso por **função** (`ROLE_USER`, `ROLE_ADMIN`).

---

## 🚀 Tecnologias Utilizadas

- ✅ Spring Boot 3
- 🔐 Spring Security (com JWT)
- 🔄 OAuth2 Resource Server
- 🧩 Spring Data JPA + H2 Database (in-memory)
- 📃 SpringDoc OpenAPI / Swagger UI
- 📈 Spring Boot Actuator + Prometheus
- 📊 Grafana
- 🧪 JUnit + Mockito + MockMvc
- 🐳 Docker
- ☁️ Render para deploy gratuito

---

## 📦 Como Rodar Localmente

```bash
# Clone o repositório
git clone https://github.com/SEU_USUARIO/NOME_DO_REPO.git
cd NOME_DO_REPO

# Rode com Maven
./mvnw spring-boot:run
```

> Por padrão, a aplicação estará disponível em: `http://localhost:8080`

---

## 🧪 Testes

```bash
# Rodar todos os testes
./mvnw test
```

- Testes de autenticação com `MockMvc`
- Testes de integração com endpoints protegidos
- Simulações de tokens expirados e inválidos

---

## 📌 Endpoints Disponíveis

### 🔐 Autenticação
| Método | Endpoint           | Descrição                         | Autenticação |
|--------|--------------------|-----------------------------------|--------------|
| POST   | `/auth/login`      | Autentica usuário e gera JWT      | ❌ Pública   |
| POST   | `/auth/register`   | Registra novo usuário             | ❌ Pública   |

### 🔒 Endpoints Protegidos
| Método | Endpoint           | Descrição                                             | Requer JWT | Restrito a |
|--------|--------------------|-------------------------------------------------------|------------|-------------|
| GET    | `/api/hello`       | Endpoint protegido genérico                          | ✅ Sim     | Qualquer usuário |
| GET    | `/api/admin`       | Acesso restrito para administradores                 | ✅ Sim     | Apenas ADMIN |

### 🧪 Testes & Dev
| Método | Endpoint              | Descrição                              | Autenticação |
|--------|-----------------------|----------------------------------------|--------------|
| GET    | `/h2-console`         | Console do banco H2                    | ❌ Pública   |
| GET    | `/swagger-ui.html`    | Interface Swagger UI                   | ❌ Pública   |
| GET    | `/v3/api-docs`        | Definição OpenAPI JSON                 | ❌ Pública   |

### 📊 Monitoramento e Métricas
| Método | Endpoint                | Descrição                               | Autenticação |
|--------|-------------------------|-----------------------------------------|--------------|
| GET    | `/actuator/health`      | Verifica o status da aplicação          | ❌ Pública   |
| GET    | `/actuator/metrics`     | Exibe todas as métricas                 | ✅ Protegido |
| GET    | `/actuator/prometheus`  | Endpoint de scraping para Prometheus    | ✅ Protegido |

---
## 🛠️ Configuração (application.yml)

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password:
  h2:
    console:
      enabled: true
      path: /h2-console
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
  devtools:
    restart:
      enabled: true

jwt:
  secret: umaChaveSecretaMuitoLongaEComplexaParaAssinarTokensJWT
  expiration: 3600000

springdoc:
  swagger-ui:
    path: /swagger-ui.html
    disable-swagger-default-url: true
  api-docs:
    path: /v3/api-docs
  info:
    title: API JWT com Spring Boot
    description: API para autenticação e autorização com JWT
    version: 1.0.0
    contact:
      name: Gabriel Koch
      email: gabriel@email.com
      url: https://github.com/Gabriel-Koch-Nunes
    license:
      name: Apache 2.0
      url: https://www.apache.org/licenses/LICENSE-2.0.html
    terms-of-service: https://Gabriel.com/termos
  external-docs:
    description: Documentação externa da API
    url: https://github.com/Gabriel-Koch-Nunes/jwt-api

management:
  endpoints:
    web:
      exposure:
        include: health, metrics, prometheus
  endpoint:
    health:
      show-details: always

```
## 🛠️ Arquitetura

├── controller/
│   └── AuthController.java
├── service/
│   ├── AuthService.java
│   └── JwtService.java
├── model/
│   └── User.java
├── repository/
│   └── UserRepository.java
├── config/
│   └── SecurityConfig.java
├── dto/
│   └── RegisterRequest.java
├── AuthIntegrationTests.java
├── application.yml
├── Dockerfile
├── pom.xml

```

## 📄 Documentação Swagger

- Interface Swagger: [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
- OpenAPI JSON: [http://localhost:8080/v3/api-docs](http://localhost:8080/v3/api-docs)

---

## 🐳 Docker

### 🔧 Dockerfile

```dockerfile
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 📦 Build e Run

```bash
# Gerar o .jar
./mvnw clean package

# Build da imagem Docker
docker build -t jwt-api .

# Rodar container
docker run -p 8080:8080 jwt-api
```

---

## ☁️ Deploy na Render

1. Acesse [https://dashboard.render.com](https://dashboard.render.com)
2. Crie um novo serviço web (Web Service)
3. Conecte seu repositório do GitHub
4. Configure:
   - **Build Command**: `./mvnw clean package`
   - **Start Command**: `java -jar target/*.jar`
   - **Environment**: `Java 17`

---

## 📈 Monitoramento com Prometheus + Grafana

- Configure o Prometheus com o endpoint:
  ```
  scrape_configs:
    - job_name: 'jwt-api'
      static_configs:
        - targets: ['localhost:8080']
  ```
- No Grafana, use o Prometheus como data source e crie dashboards com as métricas da aplicação.

---

## 📬 Contato

- **Gabriel Koch**  
  [🔗 GitHub](https://github.com/Gabriel-Koch-Nunes)  
  [📧 Email](mailto:gabriel@email.com)

---

## 📜 Licença

Este projeto está licenciado sob a [Licença Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0.html).
