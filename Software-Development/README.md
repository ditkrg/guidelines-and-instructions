# A Developer's Guide to Software Development at DIT



## Document Properties

| **Document Title**       | A Developer's Guide to Software Development at DIT    |
| :----------------------- | ----------------------------------------------------- |
| **File name**            | A Developer's Guide to Software Development at DIT.md |
| **Version**              | 1.0.0                                                 |
| **Status**               | Ratified                                              |
| **Initial Draft Date**   | August 23, 2021                                       |
| **Current Version Date** | September 29, 2021                                    |
| **Author**               | Brusk Awat Mustafa                                    |



## Reviews

| Reviewer             | Date of Review   |
| -------------------- | ---------------- |
| Shkar T. Noori | October 10, 2021 |



## Change History

| Version | Date             | Description | Affected Pages |
| ------- | ---------------  | ----------- | -------------- |
| 1.0.0   | August 23, 2021  | Draft       | All            |
| 1.0.0   | November 6, 2021 | Ratified    | All            |



[TOC]



## General Software Development Requirements

### Use of existing tools

In the development of different software solutions, it is recommended to use well-known and community-supported open-source libraries, tools, and projects that are preferably under the MIT Software License. We encourage developers to move away from packages that: 

1. Have not been updated in the past 18 months.
2. Have several open issues that are left unresolved for more than 6 months
3. Have not been adopted as a solid solution by the community.

On GitHub, we advise checking the number of stars and forks before deciding to move with an open-source library. 

Close-sourced libraries require internal approval. We do not allow closed-source software to be used without a pre-approved process. If the necessity for such a use case arises, consult with the Head of Digital Development and the Head of DevOps at DIT.



### Separation of Environments


Information and configuration must not be hardcoded in the software codebase whatsoever (i.e. Database connection string, external service URLs, IP Addresses, host names, etc). These configurations must be designed in such a way that it could be set to different values in different environments. 

Any software code that deviates from this rule will be considered as a **script** and will not go through a regular CI/CD and Deployment Pipelines. The usage of such a software requires the consent of the Head of Software Development **and** the Head DevOps Team. 



These configurations must be properly documented in two ways: 



1. In a README.md file that must exist in every project, preferably with a table that includes the following columns: ENV NAME, DESCRIPTION, DEFAULT VALUE(s)
2. In a .env.example file that can be turned to a “.env” file just be renaming it. This means that all configurations that can have default values will already include the values with them. 



Unless these documents are submitted, the code is not **accepted** and **delivery** is not complete. 

The documentations, as well as the code base, must not include any **secrets**. 

As for naming convention, the following format must be used:

1. Group environment variable configurations by appending double underdashes (__). For instance, this is how database-related environment variables can be grouped:

   ```bash
   DB__CONNECTION=
   DB__USERNAME=
   DB__PASSWORD=
   DB__DBNAME=
   ```

   Think of this structure as JSON Object keys: 

   ```json
   {
     "DB": {
       "CONNECTION": "",
       "USERNAME": "",
       "PASSWORD": "",
       "DBNAME": ""
     }
   }
   ```

   Follow the same pattern if you would like to nested objects deeper. 

2. If you environment variable is an array, append the two underdashes and the index of the position to the name. For example, consider this case: 

   ```bash
   MONGODB__REPLICA_URL__0=
   MONGODB__REPLICA_URL__1=
   MONGODB__REPLICA_URL__2=
   ```

   This is equivalent to the following JSON Object structure: 

   ```json
   {
     "MONGODB": {
       "REPLICA_URL": ["", "", ""]
     }
   }
   ```



### Documentation

Unless otherwise specified by the Head of Digital Development, the following documentations are more or less required to be part of every software code: 

1. Pre-development documentations that are created or managed by the Business Intelligence and Requirement Analysis Team of DIT. These include software requirement analysis documents, process mappings, workflow diagrams, and additive resources that resulted in the specifications of the current software version. 
2. Software Specification Document that describes the nature and detailed features of the application. 
3. Final Quality Assurance Report on Testing and Acceptance.
4. Manuals
   1. Installation Manuals
   2. Operation Manuals
   3. User Manuals
5. Release Notes 
6. README.md that describes each software code's settings. (Refer to README.md templates for each code base)



### Consistency

Applications must be built in a consistent manner from the perspective of the end-user. In case if the need of an inconsistent behavior may arise, it must be properly communicated by the Head of Software Development Team for an approval. Unapproved inconsistencies are not accepted. This must also be respected when adding new features, making changes and improvements, or fixing bugs to existing applications. 



### Compatibility

All software must be backward-compatible by default and must take previous versions into considerations. If this not possible or for some design reasons need to change, an approval from the Head of Digital Development Team must be obtained. Unapproved changes that do not adhere to this principle will be considered a bug.



### Acceptance



### Deadlines

All agreed deadlines must be kept as they are unless it is specified or extended explicitly by the Head of Digital Development Team. <u>Members of the Digital Development Team</u> are not obligated to accommodate for requests for late-term changes in the requirements. 

In case if priorities change and the agreed deadlines cannot be met, it must be properly communicated to the Head of Digital Development Team in order to come up with a new agreement. 

Each release development must come with a time plan that is approved by the Head of Digital Development Team.



### Versioning

We follow the [Semantic Versioning 2.0.0](https://semver.org/) standards to version releases. Accordingly, software applications must be versioned with the following pattern: `MAJOR.MINOR.PATCH`.

#### Releases

Releases must be versioned with the same rules of Semantic Versioning while making sure that each release receives a unique version number.

#### Web Applications

All web applications must be versioned appropriately and conform to the rules of this section. The version number must be displayed on the application's user interface unless it is specified and agreed by the Head of Digital Development Team. Manifest files in the code bases must be updated to reflect the release version. 

#### CLI Applications

In case of command line applications, an option must be provided (suggestion –v) to display the version.



### Version Control and Repository Management

All code must be controlled using Git. The code bases must be stored in independent repository in a single-application-per-repository fashion on GitHub. No repository shall contain more than one software component. 

#### Git

All software code bases must have a branch named `main` that acts as the default branch. Changing the default branch to be something else is not regularly permissible unless authorized by the Head of DevOps. 

##### Branching

Conventionally, each code base must also have a branch that corresponds to an identical deployment environment. For example, the branch `dev` corresponds to codes running in a `dev` environment, `staging` to `staging`, `pre-prod` to `pre-prod`, and `main` to `production`. 

All code bases must have a branch that corresponds to a major version. For example branch `v1.x.x` must contain all initial codes. The next major release must also get its own branch `v2.x.x` which is typically created off `main`.  (Edge cases must be explicitly authorized by the Head of Digital Development or DevOps). 

The order from which branches are merged generally takes the following form:

`major version branch` -> `dev` -> `staging` -> `pre-prod` -> `main`

An example:

`v1.x.x` -> `dev` -> `staging` -> `pre-prod` -> `main`

All the minor releases and patches within the same version follows the same flow. However, in the next major release, it takes the next form:

From `main` create `next version branch` -> `dev` -> `staging` -> `pre-prod` -> `main`

An example:

From `main` create `v2.x.x` -> `dev` -> `staging` -> `pre-prod` -> `main`

##### Merging and Pushing

All developers are free to push to the release branch and `dev` whenever they would like to. However, pushing to any other branch requires a Pull Request that needs review and approval. 

Below is an overview of the policies:

| Branch Name              | Authorized Parties                                           | Review Policy                             |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------- |
| Release / Feature Branch | No restriction                                               | No review required                        |
| dev                      | No restriction                                               | No review required                        |
| staging                  | Authorized by Head of Digital Development or DevOps  <br />Team Leads | No review required                        |
| pre-prod                 | Authorized by Head of Digital Development or DevOps          | At least 1 review by Head of DD or DevOps |
| main                     | Only Head of DevOps                                          | At least 1 review by Head of DD           |

##### Secrets

#### GitHub

All repositories must be hosted on the [KRG's main GitHub organization](https://github.com/ditkrg). All members of the Digital Development Team and the DevOps Team are eligible to receive a free seat on the organization. Contractors' collaborators must be added as external collaborators. 

All members of the organization are required to use:

1. SSH Keys for authentication.
2. GPG to sign their commits. 

Unverified/unsigned commits are not going to be allowed. 

Please refer to [this guide](https://docs.github.com/en/authentication/managing-commit-signature-verification) to setup GPG keys. 

## Distributed Computing Specifications

The guides below aim to clarify principles and design decisions when creating distributed systems. Whether the software you are building is distributed or centralized, it is important to follow the points mentioned in this sections. This will make sure that any centralized software can later be transformed into a distributed system without much headache.

If it is absolutely necessary to avoid conforming to the principles of this section, communicate the matter with the Head of Digital Development to obtain an approval. Otherwise, conformity to this section is mandatory. 



### Deployment Strategy

All software at DIT is packaged using Docker and will be running on Kubernetes. That being said, each piece of software must contain these valid documents:

1. Dockerfile: Specifies instructions of how to build the docker image for the software. 
2. .dockerignore: Specifies which files must be ignored when building the docker image.



#### Dockerfile

The Dockerfile is a standard file that gives out instruction on how to build an image for the source code. Diffberent technology stacks have different base images on the top of which the new image is built. DIT has a local registry for all needed images. The base images used in this Dockerfile must be coming from our local registry. If the on-prem registry does not contain the base images that you need, speak to the Head of DevOps so that they are made available. If it is absolutely necessary to use an image that for some reasons cannot be hosted on our local registry, speak to the Head of DevOps to obtain approval. Otherwise, the change is considered a bug. 

See also: [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

##### Docker Layer Caching 

Docker creates container images using layers. Each command that is found in a `Dockerfile` creates a new layer. Each layers contains the filesystem changes of the image between the state before the execution of the command and the state after the execution of the command.

Docker uses a layer cache to optimize the process of building Docker images and make it faster.

Docker Layer Caching mainly works on `RUN`, `COPY` and `ADD` commands.

Image layers in the Dockerfile must be properly cached and necessary techniques to invalidate the cache must be put in place. Here is an example of how caching the layers could be achieved:


```dockerfile
FROM reg.dev.krd/library/ubuntu:20.04 AS base

COPY file.txt /target.txt
RUN apt-get update -qq && apt-get install -y postgresql-client
```

In this example, when building the dockerfile, any changes to the `file.txt` will result in reinstalling the dependencies; which is most likely not what we want. we can fix that by simply moving the copy command below the run command:

```dockerfile
FROM reg.dev.krd/library/ubuntu:20.04 AS base

RUN apt-get update -qq && apt-get install -y postgresql-client
COPY file.txt /target.txt
```

This way, even when we make changes to the `file.txt`, we do not have to reinstall the dependencies.

We also recommend reading the official [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/).


### Statelessness

Distributed applications must maintain statelessness all during its life cycle. This is a founding principle of all distributed systems. 

#### HTTP

By nature, HTTP is a stateless protocol. This means that the server that receives the HTTP requests blindly serve the request and does not associate requests with any previous information. There is no special treatment for different requests, unless the server does this explicitly through attaching some state to incoming requests, usually through sessions. We will elaborate this in the upcoming sections. 

#### Sessions

Application server sessions are stateful. They do attach some sort of a meaning to each request, which in turn instructs the server to treat HTTP requests differently. This is a clear violation of the statelessness principle of HTTP. We encourage you to move away from sessions that are stored on the corresponding server. 

In a distributed system where multiple instances of the same application are running to serve requests, HTTP requests that are forwarded to these instances are never guaranteed to arrive at the same instance. Therefore, a request that has initially arrived at Instance #1, may later arrive at Instance #2. If server sessions are involved, this will be problematic as the session data on Instance #1 does not exist on Instance #2, resulting in a state contention. 

The solution to this is either: 

1. Store session information in an external data store such as Redis so that it is accessible by all instances.
2. Use sticky sessions.
3. Avoid using server sessions.

We highly encourage you to use Solution #1. 

When using sticky sessions, please speak to the Head of DevOps to obtain approval on the implementation details.

#### Storage

In a distributed system, files must not be stored locally on the host machine. Rather, a storage service must be used. 

The supported storage services:
1. DIT provides an on-prem, S3-compatible blob storage service. 
2. Persistent volumes can be attached to running containers (Only When Required).

Please speak to the Head of DevOps to obtain the necessary information in this regard.


### Race Condition

Race condition is the situation where certain behavior of a system is controlled by the sequence or the timing of events; hence resulting in unintended consequences that are typically considered to be a bug. 

#### Concurrency

The primary enabler of distributed systems is concurrency and it is usually its primary headache. When creating concurrent applications, the developer must be careful with the design of that application so that it does not run into race condition as described above. This means that application designs in concurrent situations must calculate for sequence and timing properly using certain techniques, and even strategies. These include thread-safe practices, atomic operations, locking (if required).

##### Optimistic Concurrency Control (OCC)

A very common malpractice in distributed systems that ignores race conditions is the 'check-then-act' method. In light of this, two concurrent processes may try to perform an operation, but before doing so, they must check if a certain condition is met. Unbeknown to each other, both processes check and meet the condition; therefore they act. In between these checks and the act, and due to a timing issue, another process may alter the condition, unbeknown to the previous processes. Thus, resulting in a situation where the condition is not met, but the act is performed. 

Optimistic Concurrency Control resolves this by removing the check in 'check-then-act'. It rather assumes that multiple transactions can be safely performed without locking the resources. In case if two processes try to modify the same resource, the processes acts without checking pre-conditions but must check the version of the resource and rollback if it has indeed changed. 

We highly encourage considering Optimistic Concurrency Control over Pessimistic Concurrency Control (PCC) as much as possible as the implementation complexity of the later almost often beats its advantages. If the use of PCC is absolutely necessary, speak to the Head of Digital Development to obtain approval.

#### Application Decisions

##### Be Careful with Class Variables

Be careful with using class variable for checking a condition in threaded environments and resort to instance attributes to do so. Consider the following case:

```ruby
def update_without_logging(resource_id)
  Audit.disable_logging
  
  @resource = Audit.find(resource_id)
  @resource.update(some_field: 'some value')

  Audit.enable_logging
end
```



This design will eventually fall for race conditions since some other requests may be processed on the same instance of the application while `Audit.disable_logging`, which is unintended. This happens because class variables are static and are not subjected to garbage collection. The effect of this action will be extended to any other instance of the class `Audit` that is outside of the scope of the method `update_without_logging`.

To resolve this, use an instance variable:

```ruby
def update_without_logging(resource_id)
  @resource = Audit.find(resource_id)
  @resource.disable_logging
  @resource.update(some_field: 'some value')
end
```

#### Database Decisions

##### Uniqueness

Always place a unique index on columns are that supposed to contain unique values. This goes hand in hand with the previously described situations of race conditions. Applications that only check uniqueness on application level through 'check-then-act' will fall victim to race conditions and end up having inconsistent, non-unique data that is assumed to be valid. 

##### Versioning

It is always a good practice to version records so that is can also comply with the Optimistic Concurrency Control model. This requirement is not forced but it is highly recommended. 

The version column must be atomically incremented. 

#### Message Brokers

When using message brokers for inter-services communications, it is worth considering situations that would result in messages that could be consumed multiple times. To resolve this, it is generally suggested to use the [Competing Consumers Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/competing-consumers). This will make sure that messages coming from a message broker is consumed once, by design. 

##### RabbitMQ 

Using RabbitMQ's queues is considered to be a safe option by itself as it achieves the Competing Consumers Pattern out of the box. 

##### Kafka

When using Kafka as a message broker, make sure that consumers of the same application receive the same consumer-group-id so that only one instance of the consumers can receive the message, as opposed to all of them. 

### DevOps Requirements

#### Health Check

All web application are required to provide a `GET /health` endpoint that will return a `200` HTTP Response Code if: 

1. The application was bootstrapped successful and
2. Necessary connections to different data sources can be established and
3. Connection to depending services can be established

If these conditions cannot be met, the endpoint must return a `503` HTTP Response Code. 

#### Status 

All web application are required to provide a `GET /status` endpoint with a similar response schema and a `200` HTTP Response Code: 

```json
{
    "name": {
        "type": "string",
        "description": "The application name"
    },
    "version": {
        "type": "string",
        "description": "The version of the application"
    },
    "startTime": {
        "type": "string",
        "format": "date-time",
        "description": "The start time when the application was successfully booted"
    },
    "host": {
        "type": "string",
        "description": "The name of the host machine on which the application is running"
    }
}
```

Example:
```json
{
   "name":"simple-api",
   "version":"0.1.0",
   "startTime":"2021-10-28T14:49:08.06+00:00",
   "host":"simple-api-deployment-c99cdcfdf-xrfh4"
}
```



## Software Application Specifications

### Authentication

Unless specified explicitly per project, all applications that need to use an authentication scheme, must use DIT's Central Authentication Service (CAS) for that purpose. 

#### URLs

Use __https://auth.dev.krd__ in `dev`, `staging`, and `pre-prod` environment. 

Use __https://account.id.krd__ in production environment. 

#### OpenID Connect

The CAS is an OpenID Connect-compatible service. It strictly follows the specifications of OpenID Connect to perform authentication. 

##### Discovery

It provides a discovery endpoint for dynamic information discovery at the endpoint `/.well-known/openid-configuration`

#### OAuth 2.0

The CAS is an OAuth 2.0-compatible service. It strictly follows the specifications of OAuth 2.0 to perform authorization.

##### Flows

Among the different authorization flows supported by our CAS, we only permit the followings to be used:

* Authorization Code with PKCE
* Implicit 
* Client Credentials

###### Authorization Code with PCKE 

When using this flow, we prohibit access tokens to be stored on the client-side due to security concerns. All access tokens, refresh tokens, and ID tokens must be concealed and be kept from the browser or any other client that is prone to exposure. 

For that, we follow the Backend for Frontends Pattern to safely store access tokens. Take a look at our [Backend for Frontends](https://github.com/ditkrg/Backend-for-Frontends-Template) implementation, which we mainly use for this flow. 

If you are about to use this flow, speak to the Head of Digital Development and DevOps to set the Backend for Frontends according to your needs. Otherwise, it is considered to be an application bug.

#### Authentication Strategy

When using the CAS for authentication, the method through which authentication is ensured is the validation of access tokens that is received in a Authorization Bearer header. 

Access tokens from the CAS are JWTs.

When validating the access token, it is important to check:

1. `iss` claim: the Issuer, which corresponds to the URLs in this section. 
2. `exp` claim: the expiration of the token. Only valid tokens must be allowed.
3. `aud` claim: the audience. This corresponds to the name of the your application as a protected resource in the CAS.
4. `alg`: the algorithm, which must be `RS256`.
5. `kid`: which is the key with which the token is signed. For this, you will need to use the JWKS endpoint (refer to Discovery in this section) to verify the public key. 

#### User On-boarding

Most applications are generally designed to deal with end-users. This means that beyond authentication strategies, applications must store user information to some extent. Since all users are centrally registered in the CAS, applications that use the CAS will delegate registration and authentication to it. Accordingly, applications will need a way to make sure that only specified users can access the given applications. Apparently, not all users registered in the CAS can access all applications that utilize the CAS. 

##### CAS API

For that, applications may request to search in the CAS in order to on-board users to their system using the CAS API.

###### URLs

Use __https://api.auth.dev.krd__ in `dev`, `staging`, and `pre-prod` environment. 

Use __https://api.account.id.krd__ in production environment. 

###### Authentication Flow

To authenticate your application to use the CAS API, use a `client credentials` flow with a Client ID and a Client Secret. Request a development Client ID & Secret from the Head of DevOps. 

###### Required Scopes

To use the CAS API for user on-boarding, two scopes are required:

* adminui-api-wrapper
* adminui-api-wrapper.users.search

###### Endpoints

Once you have a valid access token, you can send the token as a Authorization Bearer header. [Checkout the Swagger Documentations of the CAS API](http://api.auth.dev.krd/swagger)

### API Documentations

We expected all APIs to be documented using Swagger OpenAPI Version 3+.

### Internationalization and Localization

### Automated Testing

### Monitoring and Error Tracking

### Logging

### Pagination

### Idempotent Requests

## Accepted Technologies and Development Stacks 

### Software Development Stacks

#### Option #1 (Alpha): The Overnight Master****

1. Ruby Programming Language (version >= 2.2)
2. Ruby on Rails Framework (version >= 5)
3. Relational Databases
   1. Postgresql (version >= 11) or 
   2. MySQL (version >= 8) / Maria DB (version >= 10)
4. NoSQL Database
   1. MongoDB (version >= 4) or
   2. Elasticsearch (version >= 6)
   3. Redis (version >= 5)
5. Application server
   1. Phusion Passenger (version >= 6)
   2. Puma (version >= 3)
6. Web Server
   1. Nginx (version >= 1.18)



#### Option #2 (Beta): The Veteran

1. PHP (version >= 7)
2. Laravel (version >= 7)
3. Relational Databases:
   1. MySQL (version >= 8) / Maria DB (version >= 10)
4. NoSQL Database
   1. Redis (version >= 5)
5. Web & Application Server:
   1. Apache (version >= 2)
   2. Nginx (version >= 1.18)



#### Option #3 (Gamma): The Dilemma 

1. Elixir (version >= 1.9) / Erlang Programming Language (version >= 22)
2. Phoenix Framework (version >= 1.5)
3. Relational Databases:
   1. Postgresql (version >= 11) 
4. NoSQL Database:
   1. MongoDB (version >= 4) 
5. Application Server
   1. Cowboy (version 1.0)
6. Web Server (if needed):
   1. Nginx (version >= 1.18)



#### Option #4 (Delta): The Polymath Developer

1. Python Programming Language (version >= 2.7.13)
2. Django Framework (version >= 3)
3. Relational Databases:
   1. Postgresql (version >= 11) or 
   2. MySQL (version >= 8) / Maria DB (version >= 10)
4. NoSQL Database:
   1. MongoDB (version >= 4)  or
   2. Elasticsearch (version >= 6)
   3. Redis (version >= 5)
5. Application server:
   1. WSGI
   2. ASGI
6. Web Server:
   1. Nginx (version >= 1.18)



#### Option #5 (Epsilon): Iron Dome

1. C# Programming Language (version >= 8)
2. Frameworks:
   1. ASP.NET Core (version >= 3)
   2. ASP.NET MVC (version >= 5)
3. Relational Databases: 
   1. Entity Framework is a must
   2. MSSQL (version >= 14)
   3. Postgresql (version >= 11) or 
   4. MySQL (version >= 8) / Maria DB (version >= 10)
4. NoSQL Databases:
   1. MongoDB (version >= 4)  or
   2. Elasticsearch (version >= 6)
   3. Redis (version >= 5)
5. Web & Application Servers:
   1. Kestrel
   2. HTTP.sys



#### Option #6 (Zeta): The Enterprise Beast

1. Java Programming Language (version >= 8)
2. Runtime Environment
   1. JDK version (version >= 14)
3. Spring Framework / Spring Boot (version >= 5)
4. Relational Databases: 
   1. MySQL (version >= 8) / Maria DB (version >= 10)
   2. Postgresql (version >= 11)
5. NoSQL Databases:
   1. MongoDB (version >= 4)  or
   2. Elasticsearch (version >= 6)
   3. Redis (version >= 5)
6. Application Server:
   1. Tomcat (version >= 8)
7. Web Server:
   1. Nginx (version >= 1.18)



#### Option #7 (Eta): The Mighty Node

1. Programming Languages
   1. JavaScript Programming Language ( >= ECMAScript 6) 
   2. TypeScript Programming Language (version >= 3)
   3. Babel is a must
2. Frameworks:
   1. Nest JS (version >= 7)
   2. Express (version >= 4)
   3. Fastify (version >= 3)
   4. Meteor JS (version >=10) -- For very specific types of projects that include super reactivity and offline data synchronization.
   5. Hapi JS (version >= 18)
   6. Adonis JS (version >= 4)
3. Relational Databases:
   1. Postgresql (version >= 11) 
   2. TypeORM is a must
4. NoSQL Databases:
   1. MongoDB (version >= 4)  or
   2. Only through Mongoose
   3. Elasticsearch (version >= 6)
   4. Redis (version >= 5)
5. Application Server and Environment
   1. Node JS (version >= 12)
   2. Package Managers:
      1. Yarn (latest version always)
      2. NPM (latest version always)
   3. Web Servers:
      Nginx (version >= 1.18)



#### Option #8 (theta): Speedy Go

1. Go Programming Language
2. Frameworks:
   1. Gin/Gin-Gonic
   2. Beego
   3. Echo
3. Relational Databases: 
   1. MySQL (version >= 8) / Maria DB (version >= 10)
   2. Postgresql (version >= 11)
4. NoSQL Databases:
   1. MongoDB (version >= 4)  or
   2. Elasticsearch (version >= 6)
   3. Redis (version >= 5)


### Frontend Development Stacks

#### Option #1: React

1. React (Preferably bootstrapped with `create-react-app`)
2. Programming Languages
   1. JavaScript Programming Language ( >= ECMAScript 6) 
   2. TypeScript Programming Language (version >= 3)
3. Redux
   1. Redux Toolkit (version >= 1.6)
4. React Router (version >= 6)
5. Material UI (latest version always)
6. React Hook Form (version >= 7)
   1. Yup for Schema Validation
7. Package Managers:
   1. Yarn (latest version always)
   2. NPM (latest version always)



#### Option #2: Vue.js
