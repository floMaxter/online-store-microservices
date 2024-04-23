# Online-store
Web application for an online product store based on a microservice architecture.
#### Prerequisites: [Java 21](https://jdk.java.net/21/), [Maven](https://maven.apache.org/), [PostgreSQL](https://www.postgresql.org/), [MongoDB](https://www.mongodb.com/), [Docker](https://www.docker.com/), [Keycloak](https://www.keycloak.org/), [Mockito](https://site.mockito.org/), [MockMvc](https://docs.spring.io/spring-framework/reference/testing/spring-mvc-test-framework.html), [Grafana](https://grafana.com/), [VictoriaMetrics](https://victoriametrics.com/)

## Description
This project contains the following components:
* ```product-service``` - is responsible for working with the product catalog and contains a RestController for processing requests from clients.
* ```manager-service``` - is a client that processes manager requests and sends them to the product-service.
* ```customer-service``` - is a client that processes customer request and sends them to the product-service and feedback-service.
* ```feedback-service``` - is responsible for processing customer feedback.
* ```admin-server``` - is a module for administering services: viewing health and various metrics.

#### The application is covered with tests with using MockMvc and Mockito.

## Getting Started
### 1. Clone the repository
    git clone https://github.com/floMaxter/online-store-microservices.git
    cd online-store-microservices
or download zip archive

    https://github.com/floMaxter/online-store-microservices/archive/refs/heads/main.zip
### 2. Keycloak
OAuth 2.0/OIDC is used to authorize services and authenticate users.

Start in Docker:

    docker run --name product-delivery-keycloak -p 8082:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin -v ./config/keycloak/import:/opt/keycloak/data/import quay.io/keycloak/keycloak:23.0.7 start-dev --import-realm

### 3. PostgreSQL
A PostgreSQL is used to store information about products.

Start in Docker:

    docker run --name catalog-db -p 5434:5432 -e POSTGRES_USER=catalogue -e POSTGRES_PASSWORD=catalogue -e POSTGRES_DB=catalog postgres:16

### 4. MongoDB
A MongoDB is used to store feedback from customers.

Start in Docker:

    docker run --name feedback-db -p 27017:27017 mongo:7

### 5. Victoria Metrics
This component is used to collect and store metrics from services.

Start in Docker:

    docker run --name product-delivery-metrics -p 8428:8428 -v ./config/victoria-metrics/promscrape.yaml:/promscrape.yaml victoriametrics/victoria-m

* The command cannot be run on Windows, so it is recommended to use one of the Linux distributions or run via WSL.

### 6. Grafana
This component is used for visualization and analysis of metrics collected by Victoria Metrics

Start in Docker:

    docker run --name product-delivery-grafana -p 3000:3000 -v ./data/grafana:/var/lib/grafana -u "$(id -u)" grafana/grafana:10.2.4

 * In order to output an individual collection of metrics for each user, the command is used ```"$(id -u)"```.
 * The command cannot be run on Windows, so it is recommended to use one of the Linux distributions or run via WSL.

### 7. Before starting application, you need to register Spring profiles:
* ```standalone``` - for starting all services.
