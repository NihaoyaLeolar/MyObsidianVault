---
tags:
  - 后端开发
  - springboot
---
在项目中，`StudentData`表有两百多维度的数据，所以使用JPA的数据实体类一一对应不是很方便。于是对后端的**数据库访问的持久化框架**进行初步了解。

>可以在同一个项目中同时使用JDBC和JPA，根据具体的需求和复杂性来选择合适的数据访问方式。
这种混合使用的方法通常称为"混合模式"或"混合持久化"，可以充分发挥各自的优势。

# Spring JPA
---
>**JPA（Java Persistence API）**：JPA是Java的ORM（对象关系映射）标准，它允许将Java对象映射到关系数据库中的表。JPA更加抽象化，它使用实体类来映射数据库表，提供了一种更面向对象的方式来处理数据库。这可以减少很多样板代码，使得开发更加高效，尤其是对于标准的CRUD操作和常见的数据访问场景。
>- **Spring Data JPA** ：Spring Boot通过 Spring Data JPA提供了方便的方式来使用JPA，简化了数据访问层的开发。这种方式适用于简单的CRUD操作和通用数据访问。



# Java JDBC
---
>**JDBC**：Java数据库连接（JDBC）是用于与关系数据库进行通信的标准Java API。可以使用Spring的JdbcTemplate或其他JDBC相关类来执行SQL查询和操作关系数据库。这种方式更加灵活，适用于需要更多自定义控制和性能优化的情况。
>- **Spring Data JDBC**：Spring Data JDBC是一种用于关系数据库的轻量级数据访问框架，它不使用JPA实体类，而是将数据映射到普通Java对象。这种方式更适合对数据模型有更多自定义需求的情况。

# 其他
---
在Spring Boot中，连接关系数据库通常使用JDBC和JPA这两种主要的数据访问方式，但不仅限于这两种方式。以下是一些在Spring Boot中连接关系数据库的主要选项：

1. **JDBC**：Java数据库连接（JDBC）是用于与关系数据库进行通信的标准Java API。你可以使用Spring的JdbcTemplate或其他JDBC相关类来执行SQL查询和操作关系数据库。这种方式更加灵活，适用于需要更多自定义控制和性能优化的情况。
2. **JPA（Java Persistence API）**：JPA是Java的ORM（对象关系映射）标准，它允许将Java对象映射到关系数据库中的表。Spring Boot通过Spring Data JPA提供了方便的方式来使用JPA，简化了数据访问层的开发。这种方式适用于简单的CRUD操作和通用数据访问。
3. **Spring Data JDBC**：Spring Data JDBC是一种用于关系数据库的轻量级数据访问框架，它不使用JPA实体类，而是将数据映射到普通Java对象。这种方式更适合对数据模型有更多自定义需求的情况。
4. **MyBatis**：MyBatis是一种基于XML或注解的SQL映射框架，它与Spring Boot集成得很好。它允许你编写自定义SQL查询，并将结果映射到Java对象。
5. **其他ORM框架**：除了JPA和MyBatis之外，还有其他一些ORM框架可以与Spring Boot一起使用，如Hibernate等。
6. **Spring Data**：Spring Boot提供了Spring Data项目，它是一个用于简化数据访问的高级工具包。Spring Data支持多种数据存储技术，包括关系数据库、NoSQL数据库和其他数据存储。

根据项目需求和个人偏好，可以选择合适的数据访问方式。**通常，对于简单的数据操作和通用的CRUD功能，使用Spring Data JPA是一种便捷的方式。对于复杂的数据访问需求和自定义查询，可以选择使用JDBC、MyBatis或其他适当的工具。**Spring Boot提供了广泛的集成和配置选项，以适应不同的数据库访问场景。