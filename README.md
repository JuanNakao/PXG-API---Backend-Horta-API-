# PXG-API---Backend-Horta-API-
PXG API - Backend (Horta API)
Este é o backend do PXG API, uma aplicação que gerencia hortas utilizando uma arquitetura moderna e escalável. Ele foi desenvolvido em Java 17 com Spring Boot e segue o padrão Hexagonal Architecture (Ports and Adapters), promovendo a separação de responsabilidades e facilitando a manutenção e extensibilidade do código.

Resumo do Projeto
O backend é responsável por fornecer uma API REST para gerenciar hortas. Ele permite criar, consultar, atualizar e deletar hortas, além de realizar validações e interagir com o banco de dados. A aplicação utiliza boas práticas de desenvolvimento, como validação de dados, tratamento de exceções personalizadas e logging.

Principais Características
Arquitetura Hexagonal
Portas de Entrada (Ports In): Interfaces que definem os casos de uso da aplicação.

Portas de Saída (Ports Out): Interfaces que abstraem a interação com o banco de dados.

Adaptadores: Implementações concretas das portas de entrada e saída.

Banco de Dados
H2 (in-memory) para desenvolvimento.

Suporte para bancos relacionais como PostgreSQL em produção.

Endpoints REST
Criar, consultar, atualizar e deletar hortas.

Validação
Validação de dados de entrada com @Valid e @Validated.

Testes
Testes unitários e de integração para garantir a qualidade do código.
