# Bank Account Service

## Description
The **Bank Account Service** is a Spring Boot-based project for managing bank accounts and customers. It provides a RESTful API, GraphQL endpoints, and supports native compilation using GraalVM for high-performance deployment. This service includes features for account creation, updating, deletion, and retrieval, and integrates Swagger for API documentation.

---

## Features

1. **REST API**:
    - Full CRUD operations for `BankAccount` and `Customer` entities.
    - Automatic exposure of repository-based endpoints using `@RepositoryRestResource`.

2. **GraphQL Support**:
    - Enables querying and mutations with a schema-based approach.
    - Endpoint: `http://localhost:8081/graphiql`.
    - Provides precise and flexible data queries using a defined `schema.graphqls`.

3. **Swagger Integration**:
    - Interactive API documentation available at `http://localhost:8081/swagger-ui/index.html`.

4. **Database**:
    - Uses an in-memory H2 database for development and testing (`jdbc:h2:mem:account-db`).

5. **Projections**:
    - Different projections (e.g., `p1` for web and `p2` for mobile) to tailor data responses for various use cases.

6. **GraalVM Native Compilation**:
    - Converts the JAR into a native executable for optimized performance and minimal startup time.
    - Ideal for serverless architectures.

---

## Architecture Overview

### Entities
- **Customer**:
    - Fields: `id`, `name`, `bankAccounts`.
    - One-to-many relationship with `BankAccount`.

- **BankAccount**:
    - Fields: `id`, `createdAt`, `balance`, `currency`, `type`, `customer`.
    - Many-to-one relationship with `Customer`.

### DTOs
- **BankAccountRequestDTO**:
    - Input for creating or updating accounts.
- **BankAccountResponseDTO**:
    - Output for returning account details.

### Repositories
- `BankAccountRepository`:
    - Exposes endpoints for operations on `BankAccount`.
    - Example: `/byType` to filter accounts by `AccountType`.
- `CustomerRepository`:
    - Provides CRUD operations for `Customer`.

### Services
- `AccountService`:
    - Handles business logic for managing accounts.
    - Methods: `addAccount`, `updateAccount`.

### GraphQL Schema
```graphql
type Query {
    accountsList: [BankAccount]
    bankAccountById(id: String): BankAccount
    customers: [Customer]
}

type Mutation {
    addAccount(bankAccount: BankAccountDTO): BankAccount
    updateAccount(id: String, bankAccount: BankAccountDTO): BankAccount
    deleteAccount(id: String): String
}

type Customer {
    id: ID
    name: String
    bankAccounts: [BankAccount]
}

type BankAccount {
    id: String
    createdAt: Float
    balance: Float
    currency: String
    type: String
    customer: Customer
}

input BankAccountDTO {
    balance: Float
    currency: String
    type: String
}
```

---

## Running the Application

### Prerequisites
- Java 17 or later.
- GraalVM installed for native compilation.
- Maven for building the project.

### Steps
1. Clone the repository.
2. Build the project:
   ```bash
   mvn clean install
   ```
3. Run the application:
   ```bash
   java -jar target/bank-account-service.jar
   ```
4. Access the endpoints:
    - REST API: `http://localhost:8081/api`.
    - Swagger UI: `http://localhost:8081/swagger-ui/index.html`.
    - GraphQL Playground: `http://localhost:8081/graphiql`.

5. Compile into a native executable:
   ```bash
   mvn -Pnative package
   ```
   This generates a lightweight executable for serverless deployment.

---

## Key Concepts

### REST API
Spring Boot simplifies API development using annotations like:
- `@RestController` for custom endpoints.
- `@RepositoryRestResource` for generic, entity-based CRUD APIs.

### GraphQL
GraphQL offers flexibility in querying data. The `schema.graphqls` file defines the structure:
- **Query**: Retrieve data.
- **Mutation**: Create, update, or delete data.

### GraalVM
GraalVM allows native compilation of the application, resulting in:
- Faster startup time.
- Smaller memory footprint.
- Suitability for serverless environments.

---

## Technologies Used
- **Spring Boot**: Framework for application development.
- **H2 Database**: Lightweight, in-memory database for testing.
- **GraphQL**: Flexible query language for APIs.
- **Swagger**: API documentation and testing tool.
- **GraalVM**: Native image generation for optimized performance.

---

## Example Use Cases
1. Retrieve all bank accounts:
    - REST: `GET /api/bankAccounts`.
    - GraphQL:
      ```graphql
      query {
        accountsList {
          id
          balance
        }
      }
      ```
2. Add a new account:
    - REST: `POST /api/bankAccounts` with `BankAccountRequestDTO`.
    - GraphQL:
      ```graphql
      mutation {
        addAccount(bankAccount: {balance: 1000.0, currency: "USD", type: "SAVING_ACCOUNT"}) {
          id
        }
      }
      ```

---

## License
This project is licensed under the MIT License.