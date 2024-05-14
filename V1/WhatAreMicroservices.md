# What are Microservices?

Microservice architecture is a design approach that structures an application as a collection of smaller, independent services. Each service implements a specific business capability and operates independently from other services within the application.

## Defining a Business Domain
Similar to bounded contexts in Domain-Driven Design (DDD), microservices aim to separate relevant models into their own domain. Each microservice should own all of its data, logic, and interfaces to access the data it contains. This encapsulation ensures that microservices can evolve over time without dependencies from other services.
Breaking an application into smaller services also enables the formation of smaller, dedicated teams of developers responsible for creating and maintaining each service. This typically allows for easier resource planning, skill-level pairing, and improved communication within each team.

## Microservice Independence
One of the key advantages of microservices is their independence, which allows teams to develop, deploy, and test each service separately. This independence also enables the selection of technologies best suited to solve the specific needs of the business domain represented by each service. Additionally, individual microservices can be scaled independently to accommodate increased workloads, whether that involves allocating more processing power, memory, servers, storage, or other resources as needed. This independent scalability allows organizations to allocate resources more efficiently, resulting in cost savings.

## Improved Fault Isolation
With microservices, if one service fails, it won't bring down the entire application. The failure is isolated to that specific service, allowing the rest of the application to continue running. This improves the overall resilience and reliability of the system.

## Increased Organizational Agility
Microservices align well with agile development methodologies. With smaller, autonomous teams responsible for individual services, organizations can respond more quickly to changing business requirements and market conditions.

## Facilitated Code and Technology Reuse
Since microservices are modular and loosely coupled, it becomes easier to reuse code and technologies across different services or even different applications within an organization.

## Enhanced DevOps and Continuous Delivery
The decoupled nature of microservices simplifies the deployment process, making it easier to adopt DevOps practices and implement continuous integration and continuous delivery (CI/CD) pipelines.

## Improved Resilience and Availability
With microservices, you can implement patterns like circuit breakers, load balancing, and redundancy more effectively, improving the overall resilience and availability of the system.

## Better Utilization of Cloud Infrastructure
Microservices are well-suited for cloud environments, allowing you to leverage features like auto-scaling, containerization, and serverless computing more efficiently, potentially reducing infrastructure costs.

## Technology Heterogeneity
With microservices, you can choose the best programming languages, databases, and frameworks for each service, based on its specific requirements. This technology heterogeneity can help you leverage the most appropriate tools for the job.

## Easier to Adopt New Technologies
As microservices are independent and loosely coupled, it becomes easier to adopt new technologies for specific services without impacting the entire application. This facilitates innovation and keeps the codebase up-to-date with the latest advancements.

## Improved Team Autonomy and Ownership
Microservices promote team autonomy and a sense of ownership over individual services, which can lead to increased motivation, productivity, and accountability among team members.

## Benefits Summary

Microservices architecture is a popular approach for building applications that are resilient, highly scalable, independently deployable, and able to evolve quickly. By breaking down a monolithic application into smaller, decoupled services, organizations can realize significant benefits in terms of agility, flexibility, and maintainability.

## ServiceBricks Cost Savings Scenario

Consider a scenario where you are building a cloud-based system that requires storing application log messages for five years for compliance reasons. 
This data will be rarely accessed, except during audits, and may grow to gigabytes or even terabytes in size. 
Storing this infrequently used data in a high-end relational database could result in substantial costs, potentially thousands of dollars over time.
Alternatively, you could store your data in a cost-effective storage mechanism like Azure Data Tables, spending only pennies at a fraction of the cost.

These benefits and cost-saving potentials are key features of the ServiceBricks foundation. 
We have developed our platform to support multiple storage providers, both on-premises and in the cloud. 
Each service we develop includes implementations for all supported storage providers, giving our customers unprecedented choice to align with their needs and budgets.
Simply choose the provider you want for each service.
