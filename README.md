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
## Sem Maven Wrapper (usando Maven instalado localmente):

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
---

**Resposta (201 Created):**

```json
{
  "id": "123",
  "name": "Minha Horta",
  "location": "Rua das Flores, 123",
  "description": "Horta orgânica",
  "imageUrl": null
}
```

---

### 2. Buscar uma Horta por ID

**GET** `/hortas/{id}`

**Resposta (200 OK):**

```json
{
  "id": "123",
  "name": "Minha Horta",
  "location": "Rua das Flores, 123",
  "description": "Horta orgânica",
  "imageUrl": null
}
```
---

### 3. Atualizar uma Horta

**PUT** `/hortas/{id}`

**Corpo da requisição (JSON):**

```json
{
  "name": "Horta Atualizada",
  "location": "Rua Nova, 456",
  "description": "Descrição atualizada"
}
```
---

**Resposta (200 OK):**

```json
{
  "id": "123",
  "name": "Horta Atualizada",
  "location": "Rua Nova, 456",
  "description": "Descrição atualizada",
  "imageUrl": null
```
---

### 4. Deletar uma Horta

**DELETE** `/hortas/{id}`

**Resposta (204 No Content):**  
Sem corpo.

---
---

## ⚙️ Componentes do Projeto

### 1. Entidade Horta

Representa o **modelo de domínio** da aplicação:

```java
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

@Builder
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Horta {
    private String id;
    private String name;
    private String location;
    private String description;
    private String imageUrl; // Preparado para lógica futura de upload de imagens
}
```
---

### 2. Porta de Saída (HortaRepositoryPortOut)

Define as **operações que o repositório deve implementar**:

```java
public interface HortaRepositoryPortOut {

    Horta findById(String id);
    Horta save(Horta domain);
    void deleteById(String id);
    boolean existsByName(String name);
    Horta update(Horta domain);

}
```
---
### 3. Serviço (HortaService)

Implementa a **lógica de negócio** e utiliza a porta de saída para interagir com o banco:

```java
import org.springframework.stereotype.Service;

@Service
public class HortaService implements HortasPortIn {

    private final HortaRepositoryPortOut hortaRepositoryPortOut;

    @Override
    public HortaResponseDTO createHorta(CreateHortaRequestDTO createHortaRequestDTO) {
        Horta horta = mapHortaRequestCreate(createHortaRequestDTO);
        horta = hortaRepositoryPortOut.save(horta);
        return mapHortaResponse(horta);
    }
}
```
---

### 4. Controlador REST (HortaControllerImpl)

Exposição dos **endpoints REST**:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/hortas")
public class HortaControllerImpl implements HortaController {

    private final HortasPortIn hortasPortIn;

    @PostMapping(consumes = "application/json", produces = "application/json")
    public ResponseEntity<HortaResponseDTO> createHorta(@RequestBody CreateHortaRequestDTO hortaCreateRequestDTO) {
        HortaResponseDTO responseDTO = hortasPortIn.createHorta(hortaCreateRequestDTO);
        return ResponseEntity.status(HttpStatus.CREATED).body(responseDTO);
    }
}

```

---

## 🧪 Testes

### Testes Unitários

Os testes unitários cobrem os **serviços e repositórios**, verificando a lógica de negócio e as interações com o banco.

---

### Testes de Integração

Os testes de integração verificam os **endpoints REST**.  
Exemplo de teste para criar e buscar uma horta:

```java
import org.springframework.boot.test.context.SpringBootTest;
import org.junit.jupiter.api.Test;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HortaIntegrationTest {

    @Test
    public void createAndFetchHorta() {
        // Testa criação e busca de uma horta
    }
}
```
---

## 🗄️ Banco de Dados

### Ambiente de Desenvolvimento

- **H2 (in-memory):** O banco de dados é volátil e reiniciado a cada execução.  
- **Console H2:** disponível em [http://localhost:8080/h2-console](http://localhost:8080/h2-console)  
- **Usuário:** `SA`  
- **Senha:** (em branco)  

---

### Ambiente de Produção

Para produção, recomenda-se usar um banco relacional como **PostgreSQL**.  

**Passos para configurar:**

Adicionar a dependência no `pom.xml`:

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>

```
---

## ⚙️ Configuração do Banco de Dados no `application.yml`

Para produção, atualize o arquivo `application.yml` com as credenciais do banco PostgreSQL:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://<HOST>:<PORT>/<DATABASE>
    username: <USERNAME>
    password: <PASSWORD>
  jpa:
    hibernate:
      ddl-auto: update
```
---

## ✅ Boas Práticas

- **Arquitetura Hexagonal:** O projeto segue o padrão **Ports and Adapters**, separando lógica de negócio (domínio) de infraestrutura.  
- **Validação:** As requisições são validadas com anotações como `@Valid` e `@Validated`.  
- **Exceções Personalizadas:** Exemplo: `ResourceNotFoundException` para tratar erros de recursos não encontrados.  
- **Logs:** Utiliza `@Slf4j` para logging estruturado.

---

## 🏁 Conclusão

Este backend foi projetado para ser **modular, escalável e fácil de manter**.  
Ele utiliza boas práticas de desenvolvimento, como **arquitetura hexagonal**, validação de dados e tratamento de exceções.  

Além disso, está preparado para ser integrado com **bancos de dados relacionais** e possui uma base sólida para futuras extensões, como **upload de imagens** ou **novos endpoints**.

---


