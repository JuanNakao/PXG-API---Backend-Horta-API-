---

# PXG API - Backend (Horta API)

Este projeto é o backend do **PXG API**, uma aplicação que gerencia hortas utilizando uma arquitetura moderna e escalável.  
Ele foi desenvolvido em **Java 17** com **Spring Boot** e segue o padrão **Hexagonal Architecture (Ports and Adapters)**, promovendo a separação de responsabilidades e facilitando a manutenção e extensibilidade do código.  

---

## 📖 Resumo do Projeto

O backend é responsável por fornecer uma **API REST** para gerenciar hortas.  
Ele permite criar, consultar, atualizar e deletar hortas, além de realizar validações e interagir com o banco de dados.  

A aplicação utiliza boas práticas de desenvolvimento, como:  
- Validação de dados  
- Tratamento de exceções personalizadas  
- Logging estruturado  

---

## 🚀 Principais Características

### 🔹 Arquitetura Hexagonal
- **Portas de Entrada (Ports In):** Interfaces que definem os casos de uso da aplicação.  
- **Portas de Saída (Ports Out):** Interfaces que abstraem a interação com o banco de dados.  
- **Adaptadores:** Implementações concretas das portas de entrada e saída.  

---

### 🔹 Banco de Dados
- **H2 (in-memory):** para desenvolvimento.  
- **PostgreSQL:** suporte para ambiente de produção.  

---

### 🔹 Endpoints REST
- Criar, consultar, atualizar e deletar hortas.  

---

### 🔹 Validação
- Validação de dados de entrada com `@Valid` e `@Validated`.  

---

### 🔹 Testes
- Testes **unitários** e de **integração** para garantir a qualidade do código.  

---

---

## 📂 Estrutura do Código

A aplicação está organizada em pacotes que seguem o padrão de **arquitetura hexagonal**:

```text
src/
├── main/
│   ├── java/com/leleco_dev/PXG_API/
│   │   ├── api/rest/hortas/         # Controladores REST
│   │   ├── core/
│   │   │   ├── application/service/ # Serviços de aplicação (lógica de negócio)
│   │   │   ├── domain/entity/       # Entidades de domínio
│   │   │   ├── exception/           # Exceções personalizadas
│   │   │   ├── port/in/             # Interfaces de entrada (casos de uso)
│   │   │   ├── port/out/            # Interfaces de saída (repositórios)
│   │   ├── PXGApiApplication.java   # Classe principal da aplicação
│   ├── resources/                   # Configurações e scripts do banco
├── test/                            # Testes unitários e de integração
```

---

## 💻 Requisitos Locais

Certifique-se de ter os seguintes requisitos instalados no ambiente local:

- **Java 17 (JDK)**
- **Maven** (ou usar o wrapper `mvnw` incluso)
- **Banco de dados H2** (in-memory para desenvolvimento)

---

## ⚡ Configuração Rápida

1. **Clonar o repositório e entrar na pasta do backend:**

```bash
git clone <SEU_REPO_URL>
cd BACK/HORTA-API
```
## Build e execução:

Usando Maven Wrapper (mvnw):

```bash
./mvnw clean package
./mvnw spring-boot:run
```
##Sem Maven Wrapper (usando Maven instalado localmente):

```bash
mvn clean package
mvn spring-boot:run
```
A aplicação será iniciada em:

```bash

http://localhost:8080/PXG-API/v1
```

## 📝 Endpoints Principais

### 1. Criar uma Horta

**POST** `/hortas`

**Corpo da requisição (JSON):**

```json
{
  "name": "Minha Horta",
  "location": "Rua das Flores, 123",
  "description": "Horta orgânica"
}
```


