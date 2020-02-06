## Scheduler Application Using React & Spring Boot

### Introduction

This document describes how to create, change and cancel job scheduler with UI screens using reactJS and spring boot.

### Technology Stack

The technologies used in this application are given below. 

   **Front End**

    * ReactJS 
    * Typescript
    * Webpack

   **Back End**

    * Maven
    * Spring Boot
    * Spring Data JPA
    * Quartz Scheduler
    * H2 DB (InMemory)
    
### Prerequisite

Make sure to install the below tools and set the path in the system before proceeding to quick deployments.

* Java - JDK 1.8
* Maven Build Tool [Download][Maven]
* Node [Download][Node] - Required for development

``` maven requires JAVA_HOME variable, kindly follow the path setup given below.```

#### Windows

> set PATH=path/to/JDK/bin;%PATH%

> set JAVA_HOME=path/to/JDK

> set PATH=path/to/Maven/bin;%PATH%

#### Linux

> export PATH=path/to/JDK/bin:$PATH

> export JAVA_HOME=path/to/JDK

> export PATH=path/to/Maven/bin:$PATH

### Limitations 

In order to check the priority job execution, I have considered frequency as corn expression (String) because I have tried considering frequency as seconds (number) but there is a difference in seconds/milliseconds while trying to create the same Job frequency twice.

For Example, I have chosen 30 seconds as a frequency for JOB1

Let assume JOB1 scheduled at Wed Dec 02 16:55:48 GST 2019 and the following are subsequent execution.

| Job Name        | JOB1           |
|:------------- |:-------------|
| Priority      | 1 |
| ScheduledFireTime      | Wed Dec 02 16:56:01 GST 2019      |
| NextFireTime | Wed Dec 02 16:56:31 GST 2019      |


| Job Name        | JOB1           |
|:------------- |:-------------|
| Priority      | 1 |
| ScheduledFireTime      | Wed Dec 02 16:56:31 GST 2019      |
| NextFireTime | Wed Dec 02 16:57:01 GST 2019      |

| Job Name        | JOB1           |
|:------------- |:-------------|
| Priority      | 1 |
| ScheduledFireTime      | Wed Dec 02 16:57:01 GST 2019      |
| NextFireTime | Wed Dec 02 16:57:31 GST 2019      |

You can see jobs are running at every 30 seconds. And, I tried to schedule JOB2 with the highest priority with same 30 seconds frequency but I can't schedule due to the second's difference.

```Recommended options for testing priority job execution - Please choose 1 or 2 or 5 or 10-minute frequency```

### Not completed items from the requirement

* Sorting done only by priority ascending order and not done for enqueuing order due to some grouping issue from H2 DB. 

* get a frequency (default 0 secs, which means scheduler is disabled) - due to some limitations I have chosen corn expression.

### Source Code Download

If you have the GIT you can clone the application using the below link else use the direct download link.

> git clone https://github.com/mohancse1707/mk-scheduler-webapp.git

[Direct Download][Direct Download]

### Quick Deployment Steps

After setting Java & Maven in the path, execute the below command to start the application.

> mvn spring-boot:run

once started hit this URL: [http://localhost:8080/scheduler/#/](http://localhost:8080/scheduler/#/) 

### For Development & Contributions

Open two-terminal or command-line shell then navigate to application path and execute the below script. 

> Terminal#1: <mk-scheduler-webapp> mvn spring-boot:run

> Terminal#2: <mk-scheduler-webapp> npm install && npm run start

### Sample Screens

Server log before changing the priority jobs

![spring-boot-react](/images/blog/spring/Server-Log.JPG)

Server log after changing the priority and en queuing jobs

![spring-boot-react](/images/blog/spring/Server-Log-Priority-Update-Enqueing-NewPriority.JPG)

[Direct Download]: https://github.com/mohancse1707/mk-scheduler-webapp/archive/master.zip
[Maven]:https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.5.4/apache-maven-3.5.4-bin.zip
[Node]:https://nodejs.org/download/release/v10.15.3/



