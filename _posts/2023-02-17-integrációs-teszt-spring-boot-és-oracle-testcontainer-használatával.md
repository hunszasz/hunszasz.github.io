---
layout: post
title: Integrációs teszt Spring boot és Oracle Testcontainer használatával
date: 2023-02-17 22:33 +0100
categories: [Hasznos, SpringBoot, Test, Oracle]
---

# Cél
Integrációs tesztelési lehetőség kialakítása Oracle XE konténer használatával. 

# Teszt projekt inicializáció

![spring initializr](/assets/spring-initializr.webp)

## Használt függőségek
* Spring Web Web
* Spring Data JPA SQL
* Oracle Driver SQL
* Testcontainers Testing
* Lombok Developer Tools

# Docker telepítés
[Docker telepítés post](/hasznos/2023/02/18/docker-telepítése.html)

# Testkonténer használata
Az alábbi Maven függőségek lettek felhasználva a teszthez.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>oracle-xe</artifactId>
    <scope>test</scope>
</dependency>
```

A unit tesztet tartalmazó osztályt a ```@Testcontainers``` annotációval kell ellátni,
valamint a következő statikus mezőt kell deklarálni:

```java
@Container
public static OracleContainer oracleDB = new OracleContainer("gvenzl/oracle-xe:21-slim");
```

A properties-ek kezeléséről pedig az alábbi kódrészlet gondoskodik:
```java
@DynamicPropertySource
public static void properties(DynamicPropertyRegistry registry) {
    registry.add("spring.datasource.url",oracleDB::getJdbcUrl);
    registry.add("spring.datasource.username", oracleDB::getUsername);
    registry.add("spring.datasource.password", oracleDB::getPassword);
    registry.add("spring.datasource.driverClassName", oracleDB::getDriverClassName);
    registry.add("spring.jpa.database", () -> "oracle");
}
```
A fenti lépések elvégzése után futtathatjuk az alkalmazásunk tesztjét a TestContainer Oracle xe adatbázisra kapcsolódva.

# DB-k ellenőrzése

## H2
Webes console-t használva a http://localhost:8888/springboot-crud-rest/h2-console url-en.
jdbc:h2:mem:mydb
![h2 testcontainer](/assets/h2_testcontainer.webp)

## Oracle
A dockeren belüli oracle listener portját a ```docker ps``` paranccsal ellenőrizhetjük:
![docker ps](/assets/docker_ps_tst_container.webp)

A csatlakozáshoz használt paraméterek:
* port: az előző pont szerint
* felhasználó: test
* jelszó: test
* service name: xepdb1

![ora testcontainer](/assets/ora_testcontainer.webp)

# Hasznos linkek
* [GitHub projekt a teszthez](https://github.com/hunszasz/oracle-integration-test)
* [Testcontainer](https://www.testcontainers.org/)
* [fullstackcode.dev testcontainer post](https://fullstackcode.dev/2021/05/16/springboot-integration-testing-with-testcontainers/)