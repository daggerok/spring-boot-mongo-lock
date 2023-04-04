# distributed-lock-mongodb [![tests](https://github.com/daggerok/distributed-lock-mongodb-spring-boot-starter/actions/workflows/tests.yml/badge.svg)](https://github.com/daggerok/distributed-lock-mongodb-spring-boot-starter/actions/workflows/tests.yml) [![integration tests](https://github.com/daggerok/distributed-lock-mongodb-spring-boot-starter/actions/workflows/integration-tests.yml/badge.svg)](https://github.com/daggerok/distributed-lock-mongodb-spring-boot-starter/actions/workflows/integration-tests.yml)
A `distributed-lock-mongodb-spring-boot-starter` repository project contains custom written `Distributed Lock` starter
based on `Spring Boot` and `MongoTemplate` with `Testcontainers` integration testing and fabric8 `docker-maven-plugin`
maven module to help run example showcase application uses mongo docker container

[![Maven Central](https://img.shields.io/maven-central/v/io.github.daggerok/distributed-lock-mongodb-spring-boot-starter.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:%22io.github.daggerok%22%20AND%20a:%22distributed-lock-mongodb-spring-boot-starter%22)

```bash
./mvnw clean ; ./mvnw -U
```

## Build, run and test example application

```bash
#brew reinstall httpie jq

killall -9 java
./mvnw -f docker docker:stop
rm -rfv ~/.m2/repository/io/github/daggerok/distributed-lock-mongodb-spring-boot-starter

./mvnw clean ; ./mvnw -DskipTests install
./mvnw -f docker                                                    docker:start
./mvnw -f distributed-lock-mongodb-spring-boot-starter-example spring-boot:start

http -I get :8080/get-state/daggerok
http -I post :8080/post-state/daggerok/initialize-state

id=`http -I post :8080/post-lock/daggerok | jq -r '.id'`
http -I post :8080/post-state/daggerok/this-should-not-work
http -I post :8080/post-unlock-by-id/${id}
http -I post :8080/post-state/daggerok/but-now-this-should-work

./mvnw -f distributed-lock-mongodb-spring-boot-starter-example spring-boot:stop
./mvnw -f docker                                                    docker:stop
```

<!--

# Read Me First
The following was discovered as part of building this project:

* The JVM level was changed from '1.8' to '17', review the [JDK Version Range](https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-Versions#jdk-version-range) on the wiki for more details.

# Getting Started

### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.0.5/maven-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.0.5/maven-plugin/reference/html/#build-image)
* [Testcontainers MongoDB Module Reference Guide](https://www.testcontainers.org/modules/databases/mongodb/)
* [Testcontainers](https://www.testcontainers.org/)
* [Thymeleaf](https://docs.spring.io/spring-boot/docs/3.0.5/reference/htmlsingle/#web.servlet.spring-mvc.template-engines)
* [Spring Data MongoDB](https://docs.spring.io/spring-boot/docs/3.0.5/reference/htmlsingle/#data.nosql.mongodb)
* [Handling Form Submission](https://spring.io/guides/gs/handling-form-submission/)
* [Accessing Data with MongoDB](https://spring.io/guides/gs/accessing-data-mongodb/)

-->
