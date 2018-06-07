# Celery - Introduction

## Introduction
- an asynchronous task queue/job queue based on distributed message.
- focuses on real-time operations, but supports scheduling as well.
- execution units (tasks) are executed concurrently on one or more workers.
- tasks can be execute asynchronously or synchronously.

## Supported Brokers
- RabbitMQ
- Redis
- MongoDB
- ...

## Celery vs RabbitMQ 
- RabbitMQ (Message Brokers) receive message then deliver it to the receipients.
- Celery (Task Queue) help to manage things like producer, consumer. It allow us to define task, call task, handle task.


## RabbitMQ vs Apache Kafka
> pass