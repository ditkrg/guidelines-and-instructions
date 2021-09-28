# **A Developer's Guide to Software Development at DIT**



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



## **General Software Development Requirements**

### Use of existing tools

In the development of different software solutions, it is recommended to use well-known and community-supported open-source libraries, tools, and projects that are preferably under the MIT Software License. We encourage developers to move away from packages that: 

1. Have not been updated in the past 18 months.
2. Have several open issues that are left unresolved for more than 6 months
3. Have not been adopted as a solid solution by the community.

On GitHub, we advise checking the number of stars and forks before deciding to move with an open-source library. 

Close-sourced libraries require internal approval. We do not allow closed-source software to be used without a pre-approved process. If the necessity for such a use case arises, consult with the Head of Digital Development and the Head of DevOps at DIT.



### **Separation of Environments**


Information and configuration must not be hardcoded in the software codebase whatsoever (i.e. Database connection string, external service URLs, IP Addresses, host names, etc). These configurations must be designed in such a way that it could be set to different values in different environments. 

Any software code that deviates from this rule will be considered as a **script** and will not go through a regular CI/CD and Deployment Pipelines. The usage of such a software requires the consent of the Head of Software Development **and** the Head DevOps Team. 



These configurations must be properly documented in two ways: 



1. In a README.md file that must exist in every project, preferably with a table that includes the following columns: ENV NAME, DESCRIPTION, DEFAULT VALUE(s)
2. In a .env.example file that can be turned to a “.env” file just be renaming it. This means that all configurations that can have default values will already include the values with them. 



Unless these documents are submitted, the code is not **accepted** and **delivery** is not complete. 

As for naming convention, the following format must be used:

Group environment variable configurations by appending double underdashes (__). For instance, this is how database-related environment variables can be grouped:

```bash
DB__CONNECTION=
DB__USERNAME=
DB__PASSWORD=
DB__DBNAME=
```

Think of this structure as JSON Object keys: 

```json
{
  DB: {
    CONNECTION: "",
    USERNAME: "",
    PASSWORD: "",
    DBNAME: ""
  }
}
```

Follow the same pattern if you would like to nested objects deeper. 



