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
