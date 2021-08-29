## Microservice application with high availability

Microservice is an popular architecture for designing software application over the last few years.

This project demonstrates high availability of services using microservice architecture. 

### Features

* High availability / resilient services using ribbon load balancer
* API Gateway proxy for microservices
* JWT Authentication having configurable "time-to-live" (TTL)
* Secured write operation for product reviews
* Service to Service communications
* Reusable services for other platforms
* Separate data store for each microservice 
* Unit and Integration testing

### Application architecture

![Server](https://mohankumarrathinam.com//images/blog/spring/microservices/architecture.png)

### Technologies

The technologies used in this application are given below. 

* Java 1.8
* Spring
* Spring Boot
* H2 Database (file storage)
* Maven Build Tool [Download][Maven]

```Make sure Java and Maven is installed and set in the system path before proceeding to build & deployments```

### Limitations 

* Security enabled in API Gateway as this is the entry point for all service request, I assume that have a firewall in place, this could restrict inbound traffic to micro services and disallow anyone from accessing the microservices directly.

* If any build / compilation error occurs due the maven test execution, skip the test case execution using this command ```mvn spring-boot:run -DskipTests=true```

### Source Code Download

If you have a GIT in your system, you can clone the application using below command else use the direct download link. 

> git clone https://github.com/mohancse1707/springboot-microservice-application.git

[Direct Download][Direct Download]

## Build & Deployment

Let's get into build and deployment steps. 

```Execute mvn clean install in the root directory just to make sure application commpiled successfully```

### Run the Eureka Service Registry

In Terminal#1 Navigate to ```springboot-microservice-application\eureka-registry-service``` and execute below command 

> Terminal#1: mvn spring-boot:run

Once started hit this URL: [http://localhost:8761](http://localhost:8761) 

### Run the API Gateway Service

In Terminal#2 Navigate to ```springboot-microservice-application\api-gateway-service``` and execute below command 

> Terminal#2: mvn spring-boot:run

Once started hit this URL: [http://localhost:8080](http://localhost:8080) 

### Run the Product Service

In Terminal#3 Navigate to ```springboot-microservice-application\product-micro-service``` and execute below command 

> Terminal#3: mvn spring-boot:run

Product Service 1 URL: [http://localhost:8081](http://localhost:8081) 

```Now lets start another instance for same product service just to make sure to loadbalancing and availability of this service works fine if any system failure ```

In Terminal#4 Navigate to ```springboot-microservice-application\product-micro-service``` and execute below command 

> Terminal#4: mvn spring-boot:run -Dserver.port=8082

Product Service 2 URL: [http://localhost:8082](http://localhost:8082) 

### Run the Product Review Service

In Terminal#5 Navigate to ```springboot-microservice-application\review-micro-service``` and execute below command 

> Terminal#5: mvn spring-boot:run

Review Service URL: [http://localhost:8083](http://localhost:8083) 

Once, all services started successfully, you can see them in eureka service registry shown in below screen. 

![Server1](https://mohankumarrathinam.com//images/blog/spring/microservices/services.png)

### API Gateway Request Forwarding

| API                | API gateway request                    | Forwarded service      | Forwarded URL                      |
|--------------------|:---------------------------------------|:-----------------------|:-----------------------------------|
|Product request     |``http://localhost:8080/product/**``    | product-micro-service  | ``http://localhost:8081/** or http://localhost:8082/**``       |    
|Product review request|``http://localhost:8080/review/**``   | review-micro-service   | ``http://localhost:8083/**``       |    

### Load Balancing 

If we have multiple instance for product-service like http://localhost:8081 and http://localhost:8082. If any request with http://localhost:8080/product/** api gateway will forward it to one of the two instance of product-service as load balancing using Round-robin order since Zuul api-gateway use Ribbon load balancer. Api gateway also frequently keep updated all available instance list of a service from eureka server.

### Docker Build

* Docker files created for both product and product review service with simple build scripts.
* Docker composer file added in the root directory of this application.

### Jenkins CI CD Build

* Created simple jenkins script file for build and deployment for below services.
    - Product Service 
    - Product Review Service 

### Database Access

**Product Service**

We have launched two instances for product services, *Both instance are running as separate application, but they are using same database*

Access product service datasource in browser

Datasource URL: [http://localhost:8081/h2](http://localhost:8081/h2)  

To connect product service data source use below credentials   
JDBC URL  : `jdbc:h2:~/product`  
User Name : `root`  
Password  : `root`  

**Product Review Service**

Access product review service datasource in browser

Datasource URL: [http://localhost:8083/h2](http://localhost:8083/h2)  

To connect product review service data source use below credentials   
JDBC URL  : `jdbc:h2:~/review`  
User Name : `root`  
Password  : `root`  

### Application Testing in Postman

* Exported All endpoints from postman as JSON format located in the root directory of this application, simply take the file and import in your postman, this will reduce the time for testing all the API's.
    - `Product.postman_collection.json`
    - `Product Review.postman_collection.json`
    
* Added seed data `src/main/resources/data.sql` on launch of product service application, below are the sample data. 

    | PRODUCT_ID  | PRODUCT_TITLE      | PRICE  |
    |:------------|:-------------------|:-------|
    | H0001       | HP Elite Laptop    | 2500   |    
    | M0001       | MacBook Laptop     | 2500   |
    | L0001       | Lenovo Laptop      | 2500   |
    | D0001       | Dell Laptop        | 2500   |

***NOTES***
* To test the secured endpoints from product review first we need generate AUTH token which can be generated by a login.

    - Login endpoint : http://localhost:8080/authenticate
    - Username / Password : user / user

Take the response token (`valid for 20 min only`) add it in a header of each write operation endpoint as show below screen.

Login

![Server2](https://mohankumarrathinam.com//images/blog/spring/microservices/login.png)

Add the token in header

![Server2](https://mohankumarrathinam.com//images/blog/spring/microservices/add-heade-token.png)

Delete Review

![Server2](https://mohankumarrathinam.com//images/blog/spring/microservices/delete.png)

Add review sample JSON

```json
{
    "id": 1,
    "productId": "L0001",
    "averageReviewScore": 5,
    "numberOfReview": 40
}
```

Update review sample JSON

```json
{
    "id": 1,
    "productId": "L0001",
    "averageReviewScore": 10,
    "numberOfReview": 30
}
```

[Direct Download]: https://github.com/mohancse1707/springboot-microservice-application/archive/main.zip
[Maven]:https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.zip
