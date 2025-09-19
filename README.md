# Modernized Microservices Demo

A modernized demo application with updated Java, Go, Javascript, Kafka and PostgreSQL components using official Docker images.

## Architecture

![Architecture diagram](architecture.png)

* A front-end web app in [Java](/vote) (Spring Boot 3.2.1, Java 17) which lets you vote between Tacos and Burritos
* A [Kafka](https://hub.docker.com/r/confluentinc/cp-kafka) queue (Confluent Platform 7.5.0) which collects new votes
* A [Golang](/worker) worker (Go 1.22) which consumes votes from Kafka and stores them in PostgreSQL
* A [PostgreSQL](https://hub.docker.com/_/postgres) database (PostgreSQL 16 Alpine)
* A [Node.js](/result) webapp (Node.js 18) which shows the results of the voting in real time

## Modernization Changes

This branch (`jjc/bye-bitnami`) modernizes the application with the following updates:

### Infrastructure Changes
- **PostgreSQL**: Replaced Bitnami PostgreSQL with official `postgres:16-alpine` image
- **Kafka & Zookeeper**: Replaced Bitnami Kafka/Zookeeper with Confluent Platform (`confluentinc/cp-kafka:7.5.0`, `confluentinc/cp-zookeeper:7.5.0`)

### Application Updates
- **Vote Service**: Upgraded to Spring Boot 3.2.1 with Java 17, migrated to Jakarta EE, removed security vulnerabilities
- **Result Service**: Updated to Node.js 18 with latest secure dependencies (Express 4.18.2, Socket.io 4.7.5)
- **Worker Service**: Updated to Go 1.22 with latest IBM Sarama Kafka client and PostgreSQL driver

### Security & Performance
- Removed vulnerable dependencies (OGNL library)
- Updated all base images to latest stable versions
- Improved container build processes

## Run the demo application in Okteto

```
$ git clone https://github.com/okteto/microservices-demo-compose
$ cd microservices-demo-compose
$ okteto login
$ okteto deploy
```

## Develop on the Result microservice

```
$ okteto up -f result/okteto.yml
```

## Develop on the Vote microservice

```
$ okteto up -f vote/okteto.yml
```

## Develop on the Worker microservice

```
$ okteto up -f worker/okteto.yml
$ make start
```

## Notes

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Okteto.
