# Multi-Database Demo

A reference project demonstrating how to configure and manage **multiple relational databases within a Spring-based Java application**.

The project explores explicit persistence boundaries using multiple `DataSource` configurations, persistence contexts and transaction management.

Rather than treating persistence as a single application-wide concern, this project demonstrates how an enterprise application can maintain separate database connections and explicitly associate persistence infrastructure with the appropriate domain or data boundary.

## Overview

Many enterprise applications interact with more than one database.

This can occur when:

* Different business domains own separate data stores
* A system integrates with a legacy database
* Operational and reporting data are separated
* Applications are gradually migrated between persistence platforms
* Regulatory or organisational boundaries require data isolation
* Different persistence workloads require independent database infrastructure

A multi-database architecture introduces additional configuration and design concerns beyond a conventional single-database application.

This project explores those concerns using the Spring Framework and JPA/Hibernate.

---

# Architecture

The application demonstrates the relationship between:

```text
                         Spring Application
                                │
                ┌───────────────┴───────────────┐
                │                               │
         Persistence Boundary A          Persistence Boundary B
                │                               │
            DataSource A                    DataSource B
                │                               │
      EntityManagerFactory A        EntityManagerFactory B
                │                               │
        Transaction Manager A         Transaction Manager B
                │                               │
          Database / Schema A           Database / Schema B
```

Each persistence boundary can be configured independently.

The objective is to make database ownership explicit rather than relying on a single implicit application-wide persistence configuration.

---

# Technology Stack

| Area                | Technology                       |
| ------------------- | -------------------------------- |
| Language            | Java 17                          |
| Core Framework      | Spring Framework 6.1             |
| Persistence         | Spring Data JPA                  |
| ORM                 | Hibernate                        |
| Database            | MySQL                            |
| Persistence API     | Jakarta Persistence              |
| Transactions        | Jakarta Transactions             |
| Connection Pooling  | HikariCP / Hibernate integration |
| Logging             | SLF4J / Log4j                    |
| Testing             | JUnit 5, Mockito, Spring Test    |
| Build Tool          | Maven                            |
| Configuration Style | XML-based Spring configuration   |

The project deliberately uses explicit framework configuration to expose the underlying persistence infrastructure.



---

# Core Concept: Multiple Persistence Boundaries

A conventional Spring application often contains a single persistence configuration:

```text
Application
    │
    ▼
DataSource
    │
    ▼
EntityManagerFactory
    │
    ▼
TransactionManager
    │
    ▼
Database
```

A multi-database application introduces multiple persistence paths:

```text
Application
     │
     ├───────────────────────┐
     │                       │
     ▼                       ▼
DataSource A            DataSource B
     │                       │
     ▼                       ▼
EntityManagerFactory A  EntityManagerFactory B
     │                       │
     ▼                       ▼
Transaction Manager A   Transaction Manager B
     │                       │
     ▼                       ▼
Database A              Database B
```

The important architectural concern is therefore not simply connecting to multiple databases.

It is correctly establishing **ownership and boundaries** between the application's persistence components.

---

# Concepts Explored

This project explores:

* Multiple `DataSource` configurations
* JPA persistence configuration
* Hibernate integration
* Multiple persistence contexts
* Entity scanning
* Repository association
* Transaction management
* Connection pooling
* Database boundary separation
* Explicit Spring configuration
* Dependency injection
* Persistence infrastructure

---

# Why Multiple Databases?

Multiple databases are not automatically an architectural improvement.

They introduce additional complexity, including:

| Concern        | Architectural Question                                     |
| -------------- | ---------------------------------------------------------- |
| Data ownership | Which system or domain owns the data?                      |
| Transactions   | Can operations remain locally transactional?               |
| Consistency    | What happens when multiple databases are involved?         |
| Performance    | Are independent databases required for workload isolation? |
| Operations     | How are backups, migrations and monitoring managed?        |
| Coupling       | Are applications sharing data they should not own?         |

The purpose of this project is to explore the persistence infrastructure required when multiple database connections are necessary.

---

# Transaction Boundaries

One of the important differences between a single-database application and a multi-database application is transaction management.

Each persistence boundary may require its own transaction manager:

```text
Business Operation
       │
       ├── Transaction Manager A
       │          │
       │       Database A
       │
       └── Transaction Manager B
                  │
               Database B
```

This raises an important architectural distinction:

> **A transaction spanning multiple databases is not automatically the same as a transaction operating within a single persistence context.**

Distributed transaction coordination introduces additional complexity and should be treated as an explicit architectural decision.

---

# Configuration Approach

The project uses explicit XML-based Spring configuration.

This is intentional from an engineering-learning perspective.

Modern Spring applications frequently hide much of the underlying infrastructure behind auto-configuration. Explicit configuration makes the relationships between infrastructure components easier to inspect:

```text
Spring Context
      │
      ├── DataSource
      │
      ├── EntityManagerFactory
      │
      ├── Transaction Manager
      │
      └── Repository Configuration
```

Understanding these relationships provides a useful foundation when working with modern Spring Boot applications, where much of the same infrastructure still exists beneath auto-configuration.

---

# Project Purpose

This repository is a technical reference and learning project focused on understanding the infrastructure behind **multi-database enterprise applications**.

The objective is not simply to demonstrate CRUD operations.

The focus is on understanding how:

* Database connections are configured
* Persistence contexts are isolated
* ORM infrastructure is associated with specific databases
* Transaction boundaries are established
* Spring manages persistence infrastructure
* Multiple data stores affect application architecture

---

# Architectural Perspective

This project represents an important distinction between:

```text
"I can connect my application to a database."
```

and:

```text
"I understand how an enterprise application manages
multiple persistence boundaries and the infrastructure
required to keep them explicit."
```

The second problem involves architectural concerns around:

**ownership · isolation · transaction boundaries · persistence contexts · infrastructure configuration**


---

# Key Engineering Takeaway

The primary lesson of this project is that:

> **Multiple databases are not just a connection problem. They introduce persistence ownership, transaction and architectural boundary concerns that must be explicitly designed.**

The Spring Framework provides the infrastructure to configure these boundaries, but understanding the relationship between `DataSource`, persistence context, `EntityManagerFactory` and transaction management remains essential when designing enterprise applications.

