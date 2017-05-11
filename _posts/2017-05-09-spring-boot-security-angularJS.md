---
layout: post
title:  "Spring-Boot Application + AngularJS + Spring Security - XML Config..,"
date: 2017-05-09 00:00:00 Z
keywords: Spring, springboot, spring-boot, Security, spring security, angularjs, angular1, Java, login popup
---


Welcome to all, This is my first article, This blog is created to understand how to create a Spring-Boot application with Spring Security and AngularJS using XML configuration. 



Technologies Used are: 

* Spring-Boot 1.4.2
* Spring Security 4.3.4
* AngularJS 1.6.1
* Bower 
* Maven 3.3.9 [Download](https://maven.apache.org/download.cgi)
* Java 1.8

## AngularJS

I have choosen AngularJS for front end and Spring REST API for back-end, this the most prefered way to communicate with server from AngularJS. 

AngularJS integrated with HTML5 on front end, communicating asynchronously with server i am using angular $http.

The $http service is a core AngularJS service that facilitates communication with the remote HTTP servers via the browser's XMLHttpRequest object or via JSONP.

The $http API is based on the deferred/promise APIs exposed by the $q service. While for simple usage patterns this doesn't matter much, for advanced usage it is important to familiarize yourself with these APIs and the guarantees they provide.

AngularJS provides a great way to make single page applications. When creating single page applications, routing will be very important. We want our navigation to feel like a normal site and still not have our site refresh.

There are two routings in AngularJS

`ngRoute` is a module developed by the AngularJS team which was earlier part of the AngularJS core.

`ui-router` is a framework which was made outside the AngularJS project to improve and enhance routing capabilities.

This applications is using `ui-router` because It provides a different approach than `ngRoute` in that it changes your application views based on state of the application and not just the route URL.

### States vs URL Route

With this approach, your views and routes aren't tied down to the site URL. This way, you can change the parts of your site using your routing even if the URL does not change.

When using ngRoute, you'd have to use ngInclude or other methods and this could get confusing. Now that all of your states, routing, and views are handled in your one .config(), this would help when using a top-down view of your application.

Know more about ngRoute vs ui-route [click here][click here]

## Create a Maven Project 

Download the Maven in above link and set the path in your system. open the cmd prompt then navigate to the location where you like to create a project. Create maven project using the below command.

```java
mvn archetype:generate -DgroupId=com.mohan.sample 
-DartifactId=mk-springboot-angular 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
-Dversion=1.0.0
```

After generating the maven project, I have listed in below the required maven dependencies to add in the project. 

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.mohan.sample</groupId>
    <artifactId>mk-springboot-angular</artifactId>
    <packaging>war</packaging>
    <version>1.0.0</version>

    <name>mk-springboot-angular</name>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.2.RELEASE</version>
    </parent>

    <dependencies>

        <!--Spring Boot Depedencies -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <version>1.4.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>1.4.2.RELEASE</version>
        </dependency>

        <!--Spring Depedencies -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.4.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>4.3.4.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.4.RELEASE</version>
        </dependency>

        <!--Spring Security Depedencies -->

        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>4.0.3.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>4.0.3.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>4.0.3.RELEASE</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.6</version>
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```
## Spring Security Configuration

Create a spring security xml with inmemory user authentication as shown in below.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/security"
 xmlns:beans="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
 http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">


<http auto-config="true">
	<intercept-url pattern="/" access="permitAll"></intercept-url>
	<intercept-url pattern="/app/authentication" access="hasRole('ADMIN')" ></intercept-url>
	<form-login username-parameter="j_username"
	            password-parameter="j_password"   
	            login-page="/views/login/login.html" 
	            authentication-success-handler-ref="ajaxAuthenticationSuccessHandler"
	            authentication-failure-handler-ref="ajaxAuthenticationFailureHandler" 
	            always-use-default-target="true" default-target-url="/" 
	            login-processing-url="/app/authentication"
	            authentication-failure-url="/views/error.html?loginError=true" ></form-login>
	<csrf disabled="true"/>
	<logout logout-url="/app/logout" 
	success-handler-ref="ajaxLogoutSuccessHandler" 
	delete-cookies="JSESSIONID"  />
</http>

<authentication-manager>
    <authentication-provider>
        <user-service>
            <user name="admin"  password="admin" authorities="ROLE_ADMIN" ></user>
        </user-service>
    </authentication-provider>
</authentication-manager>

</beans:beans>
```

If you look out the abovel XML, the intercept-url pattern '/' view can be accessed all (permitAll) and the other pattern is secured with Role of ADMIN. 

If you want to hide the fact that you are using Spring Security, you should override the login-processing-url to something like "/app/authentication", as well as the username and password form parameters. This help hide the framework that you are using to secure your site.

There are multiple ways to do Authentication for a REST API – one of the defaults Spring Security provides is Form Login – which uses an authentication processing filter – [UsernamePasswordAuthenticationFilter][UsernamePasswordAuthenticationFilter]. This filter will only be invoked by the URL specified in login-processing-url and will use the value set for username-parameter and password-parameter as the username and password from the request.

The `<form-login>` element will create this filter and will also allow us to set our custom authentication success handler on it. This can also be done manually by using the `<custom-filter>`element to register a filter at the position FORM_LOGIN_FILTER – but the namespace support is flexible enough.

## Success Authentication 

Here I have used Handler classes for login and logout because a success authentication should Return 200 Instead of 301

By default, form login will answer a successful authentication request with a 301 MOVED PERMANENTLY status code. this makes sense in the context of an actual login form which needs to redirect after login. For a RESTful web service, however, the desired response for a successful authentication should be 200 OK.

This is done by injecting a custom authentication success handler in the form login filter, to replace the default one. The new handler implements the exact same login as the default `org.springframework.security.web.authentication.SavedRequestAwareAuthenticationSuccessHandler` with one notable difference – the redirect logic is removed:

## Failure Authentication 

Failed Authentication should return 401 instead of 302

Similarly we configured the authentication failure handler – the same way we did with the success handler.


A quick note, the Spring Security 4 config has changed the old defaults for XML configuration to be the same as Java defaults.

Old XML Configuration defaults – before Spring Security 4:

* loginProcessingUrl: /j_spring_security_check
* usernameParameter: j_username
* passwordParameter: j_password

Current XML Configuration defaults:

* loginProcessingUrl: /login
* usernameParameter: username
* passwordParameter: password

## Logout

A simple way to add logout configuration for spring logout functionality is defining `<logout />`. This element is enables default logout functionalities of Spring Security, The followoing URL /logout is used by default for logging out which is to be /j_spring_security_logout before Spring Security 4. 

The good approach is to redirect to login/home page when configuring the logout functionality. Spring by default redirect to the root page ('/') but this can be configurable in the namespace.

`<logout logout-success-url="/home.html" />`

However, the good idea to change the default value, to make sure that no information is published about what kind of framework has been used. 

## Prjects Structure

![spring-boot-angular](/images/blog/spring/mk-springboot-angular/spring-boot-angular.JPG)

## Download

You can download this application using git clone as shown in below

```git
    https://github.com/mohancse1707/mk-springboot-angular.git
```

If you dont have GIT in your system then download the source using the following links and extract it.   [Zip File][Zip File]

## Build and Deploy Steps

1. You should be installed NodeJs and then install bower component globally using this  cmd> `npm install bower -g`in your system because I have used bower for angular libraries.
1. Clone or Download then extract the source code.
2. Open the terminal and navigate to the souce location (ex: D:\workspace\mk-springboot-angular)
4. Install bower using this cmd> `bower install`
5. Start the server using spring boot cmd> `mvn spring-boot:run`  
6. Check the app in this URL `http://localhost:8008/`

## Demo Screen

Home Page:

![spring-boot-angular](/images/blog/spring/home-page.jpg)

Login:

![spring-boot-angular](/images/blog/spring/mk-springboot-angular/login-popup.JPG)

After Login:

![spring-boot-angular](/images/blog/spring/mk-springboot-angular/login-after.JPG)

Logout:

![spring-boot-angular](/images/blog/spring/mk-springboot-angular/logout-popup.JPG)


[UsernamePasswordAuthenticationFilter]: http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/web/authentication/UsernamePasswordAuthenticationFilter.html

[click here]: http://stackoverflow.com/questions/21023763/what-is-the-difference-between-angular-route-and-angular-ui-router

[Zip File]: https://github.com/mohancse1707/mk-springboot-angular/zipball/master