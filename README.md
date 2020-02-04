# stakater-nordmart-gateway

## Overview

A Spring Cloud Gateway application that aggregates API calls to backend services by providing a condensed REST API for front-end

## Spring Cloud Gateway

There are 2 approaches for defining routing, doing it in code or defining it in application.yaml/application.properties.
We went with having everything in configuration file. A sample configuration would look something like this

```yaml
routes:
- id: catalogService
  uri: http://localhost:8080
  predicates:
  - Path=/api/products/**
```

1. id: Unique id of route
2. uri: Hostname/path where api is running. Eg, http://localhost:8080 or http://catalog.nordmart-dev-apps.svc.cluster.local:8080
3. predicates: Condition on which routing will take place to uri/route. Eg, /api/products/** this will redirect all requests
               containing `/api/products/` URI.
Summary: `http://gateway-svc:8080/api/products/get-products` will be redirected to `http://localhost:8080/api/products/get-products`

## Configurations

Environment variables can be provided to configure the gateway.

**Service configurations**
* CUSTOMER_API_HOST - Customer service API host.
* CUSTOMER_API_PORT - Customer service API port.
* CATALOG_API_HOST - Catalog service API host.
* CATALOG_API_PORT - Catalog service API port.
* INVENTORY_API_HOST - Inventory service API host.
* INVENTORY_API_PORT - Inventory service API port.
* CART_API_HOST - Cart service API host.
* CART_API_PORT - Cart service API port.
* HTTP_PORT - Port of the gateway service.

## Dependencies

It requires following things to be installed:

* Java: ^8.0
* Spring boot 2.2.2.RELEASE
* Maven

## Deployment strategy

### Local deployment

To run the application locally use the command given below:

```bash
mvn spring-boot:run
```

### Docker

To deploy app inside a docker container

* Create a network if it doesn't already exist by executing

  ```bash
  docker network create --driver bridge nordmart-apps
  ```

* Build jar file of the app by executing

  ```bash
  mvn clean package
  ```

* Next build the image using

  ```bash
  docker build -t gateway .
  ```

* Finally run the image by executing

  ```bash
  docker run -d --name gateway --network nordmart-apps -p 8083:8080 -e CART_API_HOST=cart -e CART_API_PORT=8082 -e CATALOG_API_HOST=catalog -e CATALOG_API_PORT=8080 -e INVENTORY_API_HOST=inventory -e INVENTORY_API_PORT=8081 -e PRODUCT_SEARCH_API_HOST=search -e PRODUCT_SEARCH_API_PORT=8084 -e CUSTOMER_API_HOST=customer -e CUSTOMER_API_PORT=8085 -e HTTP_PORT=8080 gateway
  ```

### Helm Charts

#### Pre-requisites

Helm operator needs to to be running inside the cluster. Helm operator is deployed by Stakater Global Stack, deployment guidelines are provided in this [link](https://playbook.stakater.com/content/processes/bootstrapping/deploying-stack-on-azure.html)

#### Helm chart deployment

To create helm release of this application using the command given below:

kubectl apply -f [helm-release](https://github.com/stakater-lab/nordmart-dev-apps/blob/master/releases/gateway-helm-release.yaml).yaml -n <namespace-name>
