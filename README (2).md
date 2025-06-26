
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

## 🛠️ Arquitetura

```
src
├── config                # Configuração de segurança e JWT
├── controller            # Endpoints públicos e protegidos
├── model                 # Entidades JPA (User)
├── repository            # Interface JPA para acesso ao banco
├── service               # Lógica de autenticação e JWT
├── tests                 # Testes unitários e de integração
```

---

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


---

## 📦 Dependências do Projeto

Pra começar, adicione as dependências essenciais no seu `pom.xml`. As versões mais recentes do Spring Boot 3.x garantem que tudo funcione perfeitamente.

| Dependência | Descrição |
|-------------|-----------|
| `spring-boot-starter-web` | 🌐 Pra construir suas APIs RESTful. [Documentação Oficial Spring Web](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web) |
| `spring-boot-starter-security` | 🔒 O core da segurança do Spring, pra autenticação e autorização. [Spring Security Docs](https://spring.io/projects/spring-security) |
| `spring-boot-starter-oauth2-resource-server` | 🔑 Permite que sua API valide tokens JWT, agindo como um Servidor de Recursos. [OAuth2 Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html) |
| `spring-boot-starter-data-jpa` | 🗄️ Pra persistência de dados usando JPA. [Spring Data JPA Docs](https://spring.io/projects/spring-data-jpa) |
| `com.h2database:h2` | 💾 Banco de dados em memória, perfeito pra desenvolvimento e testes. [H2 Database Docs](http://www.h2database.com/html/main.html) |
| `com.auth0:java-jwt` | 🛡️ A biblioteca que você usará pra gerar e validar JWTs programaticamente. [Java JWT Docs](https://github.com/auth0/java-jwt) |
| `org.springdoc:springdoc-openapi-starter-webmvc-ui` | 📚 Gera a documentação automática da sua API com Swagger UI. [Springdoc OpenAPI](https://springdoc.org) |
| `org.springframework.boot:spring-boot-devtools` | 🛠️ Ferramentas que aceleram o desenvolvimento, como "hot reload". [Spring Boot DevTools](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.devtools) |
| `org.projectlombok:lombok` | 🍬 Reduz aquele código repetitivo (getters, setters, etc.). [Lombok](https://projectlombok.org) |
| `spring-boot-starter-test` + `spring-security-test` | ✅ Inclui JUnit 5 e Mockito, essenciais pra testes. [Spring Boot Testing](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing) |

---

## 📈 Testes de Carga com JMeter

O JMeter é essencial pra simular muitos usuários e requisições, avaliando o desempenho da sua API.

### 6.1. Instalação do JMeter

1. **Baixar:** Acesse [Apache JMeter Downloads](https://jmeter.apache.org/download_jmeter.cgi)
2. **Extrair:** Descompacte o arquivo ZIP.
3. **Executar:** Vá até o diretório `bin` e execute `jmeter.bat` (Windows) ou `jmeter.sh` (Linux/macOS).

### 6.2. Criando um Plano de Teste de Carga para Login

1. **Novo Test Plan:** `File > New`
2. **Adicionar Thread Group:** Clique com o botão direito em *Test Plan* > `Add > Threads (Users) > Thread Group`
   - Number of Threads (users): `200`
   - Ramp-up period (seconds): `20`
   - Loop Count: `10`
3. **Adicionar HTTP Request:**
   - Clique com o botão direito no *Thread Group* > `Add > Sampler > HTTP Request`
   - **Name:** Login Request
   - **Protocol:** `http`
   - **Server Name or IP:** `localhost`
   - **Port Number:** `8080`
   - **Method:** `POST`
   - **Path:** `/auth/login`
   - **Parameters:** `username=admin`, `password=123456`

---

## 🚚 Entrega do Projeto

Para a entrega deste projeto, é obrigatório que todos os arquivos-fonte estejam versionados e disponíveis em um repositório Git no GitHub. Isso inclui:

- ✅ Todo o **código-fonte Java**: Entidades, repositórios, serviços, controladores e configurações.
- ✅ Arquivo **pom.xml**: Com todas as dependências do projeto.
- ✅ Arquivo **application.yml**: Com as configurações do ambiente de desenvolvimento.
- ✅ **Testes JUnit**: Para validação de autenticação, autorização e endpoints.
- ✅ **Testes de Carga JMeter**: Arquivo `.jmx` com o plano de teste.
- ✅ **README.md** detalhado: Com instruções de uso, execução local, Swagger, monitoramento e deploy.

