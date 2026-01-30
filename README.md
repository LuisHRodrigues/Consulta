# Sistema de Consultas Médicas

## 📋 Descrição

Sistema de gerenciamento de consultas médicas desenvolvido em Spring Boot, que permite o controle completo de agendamentos, consultas, prontuários e pagamentos em um ambiente hospitalar ou clínica médica.

## 🚀 Tecnologias Utilizadas

- **Java 17**
- **Spring Boot 3.4.5**
- **Spring Data JPA**
- **Spring Web**
- **Spring HATEOAS**
- **Spring Cache (Caffeine)**
- **PostgreSQL** (banco principal)
- **MySQL** (suporte alternativo)
- **H2 Database** (testes)
- **Flyway** (migrações de banco)
- **Lombok** (redução de boilerplate)
- **SpringDoc OpenAPI** (documentação Swagger)
- **JUnit 5** e **Mockito** (testes)
- **Maven** (gerenciamento de dependências)

## 🏗️ Arquitetura do Projeto

O projeto segue uma arquitetura em camadas bem definida:

```
src/main/java/com/example/consulta/
├── config/          # Configurações (Swagger, Web)
├── controller/      # Controladores REST
├── dto/            # Data Transfer Objects
├── enums/          # Enumerações
├── hateoas/        # Assemblers para HATEOAS
├── model/          # Entidades JPA
├── repository/     # Repositórios Spring Data
├── service/        # Lógica de negócio
└── vo/             # Value Objects
```

## 📊 Modelo de Dados

### Entidades Principais

#### Usuario (Classe Base)
- **Herança**: Single Table com discriminador `dtype`
- **Campos**: id, nome, username, senha, tipo, telefone, email, dataNascimento
- **Subclasses**: Medico, Paciente, Secretaria

#### Medico
- **Herda de**: Usuario
- **Campos específicos**: crm, especialidade_id
- **Relacionamentos**: ManyToOne com Especialidade

#### Paciente
- **Herda de**: Usuario
- **Campos específicos**: cpf, cartaoSus
- **Relacionamentos**: OneToMany com Prontuario

#### Consulta
- **Campos**: id, numero, agenda_id
- **Relacionamentos**: 
  - ManyToOne com Agenda
  - OneToMany com EntradaConsulta

#### Agenda
- **Campos**: id, dataAgendada
- **Relacionamentos**: ElementCollection com horarios (List<LocalTime>)

#### Prontuario
- **Campos**: id, numero
- **Relacionamentos**: OneToMany com EntradaProntuario

#### Pagamento
- **Campos**: id, dataPagamento, valorPago, formaPagamento, status

#### Especialidade
- **Campos**: id, nome, descricao

#### Procedimento
- **Campos**: id, nome, descricao, valor

#### Exame
- **Campos**: id, nome, resultado, observacoes, consulta_id

## 🔧 Funcionalidades

### Gestão de Usuários
- **Médicos**: Cadastro com CRM e especialidade
- **Pacientes**: Cadastro com CPF e cartão SUS
- **Secretárias**: Gestão administrativa

### Gestão de Consultas
- **Criação**: Apenas médicos podem criar consultas
- **Agendamento**: Vinculação com agenda e horários
- **Entradas**: Adição de diagnósticos, tratamentos e observações
- **Visualização**: Controle de acesso por tipo de usuário

### Gestão de Prontuários
- **Histórico médico**: Registro de todas as consultas
- **Entradas detalhadas**: Diagnósticos e tratamentos
- **Vinculação**: Associação com pacientes

### Gestão de Agendas
- **Horários**: Controle de disponibilidade
- **Datas**: Agendamento por data específica
- **Disponibilidade**: Listagem de horários livres

### Gestão de Pagamentos
- **Formas de pagamento**: Múltiplas opções
- **Status**: Controle de situação do pagamento
- **Valores**: Registro de valores pagos

## 🌐 API REST

### Endpoints Principais

#### Consultas
- `POST /consulta/{idUsuario}` - Criar nova consulta
- `GET /consulta/{idUsuario}/{idConsulta}` - Buscar consulta específica
- `POST /consulta/{idConsulta}/entradas` - Adicionar entrada à consulta
- `GET /consulta` - Listar todas as consultas

#### Usuários
- `POST /usuario` - Criar usuário
- `GET /usuario/{id}` - Buscar usuário
- `PUT /usuario/{id}` - Atualizar usuário
- `DELETE /usuario/{id}` - Remover usuário

#### Médicos
- `POST /medico` - Cadastrar médico
- `GET /medico` - Listar médicos
- `GET /medico/{id}` - Buscar médico específico

#### Pacientes
- `POST /paciente` - Cadastrar paciente
- `GET /paciente` - Listar pacientes
- `GET /paciente/{id}` - Buscar paciente específico

#### Agendas
- `POST /agenda` - Criar agenda
- `GET /agenda` - Listar agendas
- `GET /agenda/{id}` - Buscar agenda específica

### Padrões da API

- **HATEOAS**: Implementação completa com links relacionados
- **DTOs**: Separação entre entrada e saída de dados
- **Validação**: Bean Validation com anotações
- **Cache**: Implementação com Caffeine
- **Documentação**: Swagger/OpenAPI integrado

## 🧪 Testes

### Estrutura de Testes

```
src/test/java/com/example/consulta/
├── controller/     # Testes de integração dos controllers
├── service/        # Testes unitários dos services
├── model/          # Testes das entidades
└── ConsultaApplicationTests.java
```

### Tipos de Testes

#### Testes de Service
- **Mocking**: Uso extensivo do Mockito
- **Cenários**: Testes de sucesso e falha
- **Validações**: Regras de negócio e permissões
- **Exemplo**: ConsultaServiceTest

#### Testes de Controller
- **MockMvc**: Testes de integração da API
- **JSON**: Serialização/deserialização
- **Status HTTP**: Validação de códigos de resposta
- **Exemplo**: ConsultaControllerTest

#### Testes de Model
- **Entidades**: Validação de relacionamentos
- **Constraints**: Testes de validação
- **Exemplo**: ConsultaTest, AgendaTest

### Cobertura de Testes

- **Controllers**: 11 classes de teste
- **Services**: 11 classes de teste  
- **Models**: 3 classes de teste
- **Frameworks**: JUnit 5, Mockito, AssertJ

## ⚙️ Configuração e Execução

### Pré-requisitos

- Java 17+
- Maven 3.6+
- PostgreSQL 12+ (ou MySQL 8+)
- IDE (IntelliJ IDEA, Eclipse, VS Code)

### Configuração do Banco

#### PostgreSQL (Padrão)
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/consultas_db
spring.datasource.username=postgres
spring.datasource.password=123
spring.datasource.driver-class-name=org.postgresql.Driver
```

#### MySQL (Alternativo)
```properties
spring.datasource.url=jdbc:mysql://localhost:3307/consultas_db?useTimezone=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

### Executando a Aplicação

1. **Clone o repositório**
```bash
git clone https://github.com/Rafadiassss/Consulta.git
cd Consulta/consulta-api/consuta
```

2. **Configure o banco de dados**
   - Crie o banco `consultas_db`
   - Ajuste as credenciais em `application.properties`

3. **Execute as migrações**
```bash
mvn flyway:migrate
```

4. **Execute a aplicação**
```bash
mvn spring-boot:run
```

5. **Acesse a documentação**
   - Swagger UI: http://localhost:8081/swagger-ui.html
   - OpenAPI JSON: http://localhost:8081/v3/api-docs

### Executando os Testes

```bash
# Todos os testes
mvn test

# Testes específicos
mvn test -Dtest=ConsultaServiceTest
mvn test -Dtest=ConsultaControllerTest

# Com perfil de teste
mvn test -Ptest
```

## 📁 Estrutura de Arquivos

### Migrações Flyway
- `V1.0__Criar_Tabelas_Iniciais.sql` - Estrutura inicial do banco
- `V2.0__Inserir_Dados_Iniciais.sql` - Dados de exemplo
- `V3.0__Atualizar_Insercao_De_Dados.sql` - Atualizações de dados

### Configurações
- `application.properties` - Configurações principais
- `SwaggerConfig.java` - Configuração da documentação
- `WebConfig.java` - Configurações web

## 🔒 Segurança e Validações

### Controle de Acesso
- **Médicos**: Podem criar e visualizar consultas
- **Pacientes**: Acesso limitado aos próprios dados
- **Secretárias**: Funções administrativas

### Validações
- **Bean Validation**: Anotações nos DTOs
- **Regras de Negócio**: Implementadas nos services
- **Integridade**: Constraints no banco de dados

## 📈 Performance

### Cache
- **Caffeine**: Cache em memória
- **TTL**: 2 horas após escrita
- **Endpoints**: Listagens frequentes

### Otimizações
- **Lazy Loading**: Relacionamentos JPA
- **Paginação**: Implementada nos repositórios
- **Índices**: Definidos nas migrações

## 🚀 Deploy

### Perfis de Ambiente
- **Desenvolvimento**: H2 Database
- **Teste**: PostgreSQL local
- **Produção**: PostgreSQL remoto

### Build
```bash
mvn clean package
java -jar target/consulta-0.0.1-SNAPSHOT.jar
```

## 📚 Documentação Adicional

- **Swagger**: Documentação interativa da API
- **JavaDoc**: Documentação do código
- **Postman**: Collection de testes disponível

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request


## 👥 Autores

- **Trabalho de WEB II**
- **GitHub**: https://github.com/LuisHRodrigues/Consulta

## 📞 Suporte

Para dúvidas ou suporte, abra uma issue no repositório do GitHub.

---

**Versão**: 0.0.1-SNAPSHOT  
**Última atualização**: 2024
