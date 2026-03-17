# CST8915 Lab 5 – Containerizing the Algonquin Pet Store

## Student
Jingjing Duan

## Lab Overview
In this lab, I containerized the Algonquin Pet Store microservices using Docker and Docker Compose.  
The application consists of multiple services that run together as containers.

Services included:

- RabbitMQ (message broker)
- Order Service (Node.js)
- Product Service (Python)
- Store Front (Vue.js + Nginx)

Docker was used to build images for each microservice, and Docker Compose was used to run the multi-container application locally.

---

# Demo Video

YouTube link:

---

# Docker Images

The Docker images are available on Docker Hub:

- https://hub.docker.com/r/duan0029/order-service
- https://hub.docker.com/r/duan0029/product-service
- https://hub.docker.com/r/duan0029/store-front

---

# Docker Compose Configuration
docker-compose.yml

```yaml
version: '3'

services:
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      - RABBITMQ_DEFAULT_USER=myuser
      - RABBITMQ_DEFAULT_PASS=mypassword

  order-service:
    build: ./order-service-L4
    ports:
      - "3001:3000"
    environment:
      - RABBITMQ_CONNECTION_STRING=amqp://myuser:mypassword@rabbitmq:5672/
      - PORT=3000
    depends_on:
      - rabbitmq

  product-service:
    build: ./product-service-python-L4
    ports:
      - "3031:3030"
    environment:
      - PORT=3030

  store-front:
    build: ./store-front-L4
    ports:
      - "8080:80"
    depends_on:
      - product-service
      - order-service
```
