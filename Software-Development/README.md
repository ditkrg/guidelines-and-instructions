# A Developer's Guide to Software Development at DIT



## Document Properties

| **Document Title**       | A Developer's Guide to Software Development at DIT    |
| :----------------------- | ----------------------------------------------------- |
| **File name**            | A Developer's Guide to Software Development at DIT.md |
| **Version**              | 1.0.0                                                 |
| **Status**               | Draft                                                 |
| **Initial Draft Date**   | August 23, 2021                                       |
| **Current Version Date** | September 29, 2021                                    |
| **Author**               | Brusk Awat Mustafa                                    |



## Reviews

| Reviewer             | Date of Review   |
| -------------------- | ---------------- |
| [Reviewer Name Here] | [Date of Review] |



## Change History

| Version | Date            | Description | Affected Pages |
| ------- | --------------- | ----------- | -------------- |
| 1.0.0   | August 23, 2021 | Draft       | All            |



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

We follow the [Semantic Versioning 2.0.0](https://semver.org/) standards to version releases. Accordingly, software applications must be versioned with the following pattern: `MAJOR.MINOR.PATCH.`  

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

Conventionally, each code base must also have a branch that corresponds to an identical deployment environment. For example, the branch `dev` corresponds to codes running in an `dev` environment, `staging` to `staging`, `pre-prod` to `pre-prod`, and `main` to`production`. 

All code bases must have a branch that corresponds to a major version. For example branch `v1.x.x` must contain all initial codes. The next major release must also get its own branch `v2.x.x` which is typically created off `main`.  (Edge cases must be explicitly authorized by the Head of Digital Development or DevOps). 

The order from which branches are merged generally takes the following form:

`major version branch` -> `dev` -> `staging` -> `staging` -> `pre-prod` -> `main`

An example:

`v1.x.x` -> `dev` -> `staging` -> `staging` -> `pre-prod` -> `main`

All the minor releases and patches within the same version follows the same flow. However, in the next major release, it takes the next form:

From `main` create `next version branch` -> `dev` -> `staging` -> `pre-prod` -> `main`

An example:

From `main` create `v2.x.x` -> `dev` -> `staging` -> `pre-prod` -> `main`

##### Merging and Pushing

All developers are free to push to the release branch and `dev` whenever they would like to. However, pushing to any branch other branch requires a Pull Request that needs review and approval. 

Below is an overview of the policies:

| Branch Name              | Authorized Parties                                           | Review Policy                             |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------- |
| Release / Feature Branch | No restriction                                               | No review required                        |
| dev                      | No restriction                                               | No review required                        |
| staging                  | Authorized by Head of Digital Development or DevOps  <br />Team Leads | No review required                        |
| pre-prod                 | Authorized by Head of Digital Development or DevOps          | At least 1 review by Head of DD or DevOps |
| main                     | Only Head of DevOps                                          | At least 1 review by Head of DD           |

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

All software at DIT is deployed using Docker and will be running on Kubernetes. That being said, each piece of software must contain these valid documents:

1. Dockerfile: Specifies instructions of how to build the docker image for the software. 
2. .dockerignore: Specifies which files must be ignored when building the docker image.



#### Dockerfile

The Dockerfile is a standard file that gives out instruction on how to build an image for the source code. Diffberent technology stacks have different base images on the top of which the new image is built. DIT has a local registry for all needed images. The base images used in this Dockerfile must be coming from our local registry. If the on-prem registry does not contain the base images that you need, speak to the Head of DevOps so that they are made available. If it is absolutely necessary to use an image that for some reasons cannot be hosted on our local registry, speak to the Head of DevOps to obtain approval. Otherwise, the change is considered a bug. 



##### Docker Layer Caching 

Docker creates container images using layers. Each command that is found in a `Dockerfile` creates a new layer. Each layers contains the filesystem changes of the image between the state before the execution of the command and the state after the execution of the command.

Docker uses a layer cache to optimize the process of building Docker images and make it faster.

Docker Layer Caching mainly works on `RUN`, `COPY` and `ADD` commands.

Image layers in the Dockerfile must be properly cached and necessary techniques to invalidate the cache must be put in place. Here is an example of how caching the layers could be achieved:



```dockerfile
FROM reg.dev.krd/phusion/passenger-full:1.0.14 AS base

ENV HOME /root
CMD ["/sbin/my_init"]

ADD nginx.conf /etc/nginx/sites-enabled/webapp.conf
RUN rm -f /etc/service/nginx/down
RUN rm /etc/nginx/sites-enabled/default
RUN mkdir /home/app/webapp
RUN nginx -t 


RUN apt-get update -qq && apt-get install -y postgresql-client
RUN gem install bundler -v 2.2.17

FROM base as installation

ENV BUNDLER_VERSION=2.2.17
```



We also recommend reading the official [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/).



### Statelessness

### Race Condition

#### Concurrency

##### Optimistic Concurrency

#### Application Architecture Design

#### Database Design

#### Message Brokers



## Software Architecture Design and Non-functional Requirements

## Software Applications Specifications

### Accepted Technologies and Development Stacks 

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
