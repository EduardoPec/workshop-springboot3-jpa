# 🛒 Course API – Sistema de Pedidos com Spring Boot 3 e JPA

![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.6-brightgreen?logo=springboot)
![Maven](https://img.shields.io/badge/Maven-Project-blue?logo=apachemaven)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## 📘 Descrição

O **Course API** é uma aplicação REST desenvolvida em **Java 17** com **Spring Boot 3.5.6**, que implementa um sistema de **gerenciamento de pedidos (Order System)**.  
O projeto simula as funcionalidades essenciais de um e-commerce: **usuários, produtos, categorias, pedidos, itens de pedido e pagamentos** — com modelagem de domínio sólida, **JPA/Hibernate**, camadas REST/Service/Repository e **tratamento global de exceções**.

---

## ⚙️ Tecnologias Utilizadas

| Tecnologia | Função |
|-------------|--------|
| ☕ **Java 17** | Linguagem principal |
| 🌱 **Spring Boot 3.5.6** | Framework backend |
| 🧩 **Spring Data JPA / Hibernate** | ORM e persistência |
| 🗃️ **H2 Database** | Banco de dados em memória (dev/test) |
| 🧰 **Maven** | Gerenciador de dependências |
| ⚡ **Spring Web** | Criação dos endpoints REST |

---

## 🧱 Estrutura de Pastas
```
course/
├── pom.xml
├── src/
│ ├── main/
│ │ ├── java/
│ │ │ └── com/
│ │ └── educandoweb/
│ │ └── course/
│ │ ├── CourseApplication.java
│ │ ├── config/
│ │ │ └── TestConfig.java
│ │ ├── entities/
│ │ │ ├── Category.java
│ │ │ ├── Order.java
│ │ │ ├── OrderItem.java
│ │ │ ├── Payment.java
│ │ │ ├── Product.java
│ │ │ ├── User.java
│ │ │ └── pk/
│ │ │ └── OrderItemPK.java
│ │ ├── enums/
│ │ │ └── OrderStatus.java
│ │ ├── repositories/
│ │ │ ├── CategoryRepository.java
│ │ │ ├── OrderItemRepository.java
│ │ │ ├── OrderRepository.java
│ │ │ ├── ProductRepository.java
│ │ │ └── UserRepository.java
│ │ ├── resources/
│ │ │ ├── CategoryResource.java
│ │ │ ├── OrderResource.java
│ │ │ ├── ProductResource.java
│ │ │ ├── UserResource.java
│ │ │ └── exceptions/
│ │ │ ├── ResourceExceptionHandler.java
│ │ │ ├── ResourceNotFoundException.java
│ │ │ ├── DatabaseException.java
│ │ │ └── StandardError.java
│ │ └── services/
│ │ ├── CategoryService.java
│ │ ├── OrderService.java
│ │ ├── ProductService.java
│ │ └── UserService.java
│ └── resources/
│ ├── application.properties
│ └── static/ (opcional)
└── test/
└── java/
└── com/
└── educandoweb/
└── course/
└── (testes automatizados)
```

---

## 🔗 Relacionamentos Entre Entidades

- **User 1 ➜ N Order**  
- **Order N ➜ N Product** *(via `OrderItem`)*  
- **Order 1 ➜ 1 Payment**  
- **Product N ➜ N Category**

Principais anotações:
```java
@OneToMany(mappedBy = "client")
@ManyToMany
@OneToOne(mappedBy = "order", cascade = CascadeType.ALL)
```

---

## 🧩 Dependências Principais

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>

  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>

  <dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
  </dependency>

  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
  </dependency>
</dependencies>
```

---

## ⚙️ Banco de Dados (H2)

O projeto utiliza o **H2 Database**, um banco de dados em memória, ideal para ambiente de testes e desenvolvimento.  
Ele é recriado automaticamente a cada execução da aplicação e populado com os dados definidos na configuração (`TestConfig.java`).

### 🔧 Configuração no `application.properties`

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

---

## 📄 Endpoints Principais

A API segue o padrão RESTful, com rotas organizadas por recurso.

| Método | Endpoint | Descrição |
|--------|-----------|-----------|
| `GET` | `/users` | Retorna todos os usuários cadastrados |
| `GET` | `/users/{id}` | Retorna um usuário pelo seu ID |
| `POST` | `/users` | Cadastra um novo usuário |
| `PUT` | `/users/{id}` | Atualiza os dados de um usuário existente |
| `DELETE` | `/users/{id}` | Remove um usuário pelo ID |
| `GET` | `/products` | Retorna todos os produtos |
| `GET` | `/categories` | Retorna todas as categorias de produtos |
| `GET` | `/orders` | Retorna todos os pedidos realizados |
| `GET` | `/orders/{id}` | Retorna os detalhes de um pedido específico |

📌 **Exemplo de resposta – `GET /users/1`**
```json
{
  "id": 1,
  "name": "Eduardo",
  "email": "eduardo@gmail.com",
  "phone": "988888888",
  "password": "12345"
}
```

---

## ⚠️ Tratamento de Exceções

O projeto implementa um **tratamento global de erros** usando a anotação `@ControllerAdvice`  
na classe `ResourceExceptionHandler.java`, garantindo respostas padronizadas em formato JSON.

### 🧱 Estrutura de Exceções

| Classe | Função |
|---------|--------|
| `ResourceNotFoundException` | Lançada quando um recurso solicitado não é encontrado (ex: ID inexistente) |
| `DatabaseException` | Lançada em casos de violação de integridade referencial ou erro de banco |
| `StandardError` | Modelo de resposta JSON utilizado em todos os erros |
| `ResourceExceptionHandler` | Classe responsável por interceptar e formatar as exceções |

---

### 🧩 Exemplo de resposta - Recurso não encontrado (404)

```json
{
  "timestamp": "2025-10-17T13:00:00Z",
  "status": 404,
  "error": "Resource not found",
  "message": "User not found",
  "path": "/users/99"
}
```

---

## 🚀 Como Executar o Projeto

Siga os passos abaixo para executar o projeto localmente na sua máquina.

---

### 🧩 Pré-requisitos

Antes de iniciar, verifique se você possui instalado:

- **Java 17** ou superior  
- **Maven 3.8+**  
- **Git** (opcional, para clonar o repositório)  
- **IDE** (IntelliJ IDEA, Eclipse ou VS Code)  

---

### 🪄 Passo a passo


1️⃣ Clonar o repositório
```
bash
git clone https://github.com/seuusuario/course-api.git
```

2️⃣ Acessar a pasta do projeto
```
cd course-api
```

3️⃣ Compilar o projeto
```
-mvn clean install
```

4️⃣ Executar a aplicação
```
-./mvnw spring-boot:run
```
```
-src/main/java/com/educandoweb/course/CourseApplication.java
```

5️⃣ Acessar a aplicação
```
http://localhost:8080
```


---

## 🧠 Conceitos Envolvidos

O projeto **Course API** foi desenvolvido aplicando diversos conceitos fundamentais de **desenvolvimento backend com Spring Boot**, **boas práticas de arquitetura** e **programação orientada a objetos**.  

Abaixo estão os principais pontos técnicos envolvidos:

### ⚙️ Arquitetura e Camadas

- **Camada de Entidades (Modelos)** → Representam as tabelas do banco de dados, com mapeamento via JPA e anotações como `@Entity`, `@Table`, `@Id`, `@OneToMany`, etc.  
- **Camada de Repositórios (Repository)** → Interfaces que herdam de `JpaRepository`, responsáveis pela persistência e consultas automáticas.  
- **Camada de Serviços (Service)** → Centraliza as regras de negócio e interações entre entidades e repositórios.  
- **Camada de Recursos (Controller/Resource)** → Responsável pelos endpoints REST da aplicação, utilizando `@RestController` e `@RequestMapping`.

---

### 🧩 Princípios e Boas Práticas

- **Injeção de Dependência** (`@Autowired`) → Facilita o acoplamento entre camadas e melhora a manutenibilidade.  
- **Padrão RESTful** → Estrutura organizada e semântica para criação de APIs HTTP.  
- **Tratamento Global de Exceções** (`@ControllerAdvice`) → Garante respostas consistentes em formato JSON para erros.  
- **Perfis de Ambiente** (`@Profile("test")`) → Permite alternar entre configurações de execução (testes, produção, etc.).  
- **Banco de Dados em Memória (H2)** → Facilita o desenvolvimento e testes locais sem necessidade de instalação externa.  
- **População Automática de Dados** → Realizada via `CommandLineRunner` na classe `TestConfig`.

---

### 🧱 Conceitos de JPA / Hibernate

- Mapeamento de **relacionamentos complexos** (1:1, 1:N, N:N)  
- Uso de **chave composta** (`@EmbeddedId`)  
- **Enumeração personalizada** (`OrderStatus`) com armazenamento numérico e controle de valores no banco  
- **CascadeType** e **orphanRemoval** para controle de persistência automática  
- **FetchType.LAZY / EAGER** para otimização de carregamento de dados

---

### 🧠 Objetivo Educacional

Este projeto tem como objetivo **consolidar fundamentos de backend moderno com Java e Spring Boot**, explorando:
- Estrutura de projetos reais;
- Boas práticas de API REST;
- Integração entre camadas;
- Persistência e relacionamentos no banco de dados;
- Tratamento de erros e respostas padronizadas.

---

✨ **Resultado:**  
Um backend completo, funcional e escalável — ideal como base para sistemas de e-commerce, gestão de pedidos ou estudos de arquitetura de software.
