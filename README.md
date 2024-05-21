![ServiceBricks Logo](https://github.com/holomodular/ServiceBricks/blob/main/Logo.png)  

[![NuGet version](https://badge.fury.io/nu/ServiceBricks.svg)](https://badge.fury.io/nu/ServiceBricks)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

# ServiceBricks Documentation

## Introduction
[ServiceBricks](https://github.com/holomodular/ServiceBricks) is a powerful microservices platform designed to streamline the development, deployment, and maintenance of distributed systems. By leveraging Domain-Driven Design (DDD), Event-Driven Architecture (EDA), and a range of advanced features, ServiceBricks empowers teams to build robust, scalable, and highly customizable services tailored to their specific business domains.

View the following sections to learn more about the ServiceBricks platform.

## Learning
* [What are Microservices](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/WhatAreMicroservices.md): Learn about microservices and their benefits.
* [ServiceBricks Key Features](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/KeyFeatures.md): Learn about the key features of the ServiceBricks platform.

## Architecture
* [Domain-Driven Design and Major System Components](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/FlowOfData.md): Describes the major system components and how data flows through the system.
* [Event-Driven Architecture](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/EventDrivenArchitecture.md): Describes how Event-Driven Architecture (EDA) is used when processing methods.
* [SQL and NoSQL Database Engine Support](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/SupportedDatabaseEngines.md): Describes the standard relational (SQL) and document (NoSQL) database engines we support and how to incorporate others.
* [Classic vs Modern REST API Design](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/ClassicVsModernRestApi.md): Describes the differences between Classic and Modern REST API Design implementations and how to configure their usage.
* [NuGet Package Architecture](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/NuGet.md): Describes the NuGet package design.
* [Deployment Scenarios](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/DeploymentScenarios.md): Describes the most common deployment scenarios for your microservices foundation.


## Features

* [API Clients, Controllers and Services](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/APIServices.md): API Controllers, clients, services, methods and policies. Classic and Modern modes of operation.
* [Background Tasks and Timers](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/BackgroundTasks.md): Background task processing using hosted services and timers in the platform.
* [Business Rules and the Business Rule Engine](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/BusinessRuleEngine.md): Create, registering and executing business rules,
* [Data Transfer Object](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/DataTransferObject.md): The Data Transfer Object (DTO) and enabling storage agnostic, repository-based APIs.
* [Broadcasts and Service Bus Communication](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/BroadcastsAndServiceBus.md): Broadcasting data using Service Bus and communicating with other microservices.
* [Domain Objects, Broadcasts, Events, Process](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/DomainObjects.md): Domain objects are the core object of the platform and how to use and extend them.
* [DomainRepository and StorageRepository](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/DomainRepositoryAndStorageRepository.md): Understanding domain business logic and storage data access.
* [Exceptions](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/Exceptions.md): How to handle exceptions in the platform.
* [Modules and AutoMapper Mappings](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/ModulesAndMappings.md): Registering a module with ServiceBricks and using AutoMapper mappings.
* [Request and Response Objects](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/RequestAndResponse.md): Request and response objects, their usage and importance.
* [ServiceQuery](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/ServiceQuery.md): Dynamically query SQL and NoSQL database engines over REST APIs.

## Getting Started with Examples
* [Examples and Quickstart Applications](https://github.com/holomodular/ServiceBricks-Examples): View all our examples to create your own applications quickly.

## Official Pre-Built Microservices
We have developed several pre-built microservices to help get you started. View the following repositories for more information:

* [Cache Microservice](https://github.com/holomodular/ServiceBricks-Cache): This repository is a temporary generic data storage microservice.
* [Logging Microservice](https://github.com/holomodular/ServiceBricks-Logging): This repository is a service-scoped or centralized application and web request logging microservice.
* [Notification Microservice](https://github.com/holomodular/ServiceBricks-Notification): This repository is a notification and delivery of emails and SMS messages microservice.
* [Security Microservice](https://github.com/holomodular/ServiceBricks-Security): This repository is an authentication, authorization and application security microservice supporting JWT token membership for all ServiceBricks microservices.

## Developing Your Own Microservices
We recommend that you start with the Cache Microservice as it is very simple key/value pair storage microservice. It contains a single DTO that maps to a single database table/collection with a couple properties. Depending on the storage provider, a primary key and other properties might be needed. Otherwise, the cache key is translated to the DTO StorageKey property.

* [Setup Development Environment](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/SetupDevelopmentEnvironment.md): In order to develop ServiceBricks microservices, these are the tools you should have installed.
* [Xunit Testing Framework](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/XunitTesting.md): Test and verify your microservice is working correctly in the platform.
* [Setup GitHub CI/CD Builds and Code Coverage](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/SettingUpBuilds.md): Setup CI/CD builds and code coverage for your microservice.

## License
ServiceBricks is released under the MIT License.
