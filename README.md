# Spring Security: 

## Authentication vs Authorization

Authentication and Authorization are often used in conjunction with each other in terms of security, especially when it comes to gaining access to the system.

Authentication means confirming your own identity, while authorization means granting access to the system. We can say, authentication is the process of verifying who you are, while authorization is the process of verifying what you have access to.

## Authentication

- While doing the authentication, we validate the credentials like user name or user id and password.
- Authentication factors determine the various elements the system uses to verify one’s identity prior to granting him access to anything from accessing a file to requesting a bank transaction.
- A user’s identity can be determined by what he knows, what he has, or what he is.

When it comes to security, at least two or all the three authentication factors must be verified in order to grant someone access to the system.

### Types of Authentication

- **Single-Factor Authentication**: It commonly relies on a simple password to grant user access to a particular system such as a website or a network.
- **Two-Factor Authentication**: It’s a two-step verification process which not only requires a username and password, but also something only the user knows, to ensure an additional level of security, such as an ATM pin or security answers, which only the user knows.
- **Multi-Factor Authentication**: This is the most advanced method of authentication which uses two or more levels of security from independent categories of authentication to grant user access to the system. All the factors should be independent of each other to eliminate any vulnerability in the system.

---
—————————————————————————————————————————————————————————————  

## Authorization :

- Authorization occurs after the identity is successfully authenticated by the system, which ultimately gives you full permission to access the resources such as information, files,
  databases, funds, locations, almost anything.
  
- Authorization is the process to determine whether the authenticated user has access to the particular resources.
  
- It verifies your rights to grant you access to resources.

- Authorization usually comes after authentication which confirms your privileges to perform.

—————————————————————————————————————————————————————————————  

## Difference Between Authentication and Authorization

- **Authentication** confirms your identity to grant access to the system. **Authorization** determines whether you are authorized to access the resources.
- **Authentication** is the process of validating user credentials to gain user access. **Authorization** is the process of verifying whether access is allowed or not.
- **Authentication** determines whether the user is what he claims to be. **Authorization** determines what users can and cannot access.
- **Authentication** usually requires a username and a password. Authentication factors required for authorization may vary, depending on the security level.
- **Authorization** is done after successful authentication.
—————————————————————————————————————————————————————————————  

### Let’s create a Spring Starter Project from STS, as of now this project has only Spring Web dependency.

<img width="1440" alt="Screenshot 2024-12-31 at 6 35 55 PM" src="https://github.com/user-attachments/assets/6ccdbfb5-37c4-42db-85a8-576ae027f27a" />


Create controller classes AuthenticationController.java and UserController.java inside it.

#### UserController.java

```java
package com.prabhat.controller;

import org.springframework.web.bind.annotation.GetMapping;

@RestController
@RequestMapping("/user")
public class UserController { 
    @GetMapping("/sayuser")
    public String sayUser(){
        return "Hello User";
    }

    @GetMapping("/echo/{message}")
    public String echoMessage(@PathVariable String message){
        if (message == null || message.isEmpty()) {
            return "Message is blank";
        }
        return message;
    }
}
```
#### AuthenticationController.java
```java
package com.prabhat.controller;

import org.springframework.web.bind.annotation.GetMapping;

@RestController
@RequestMapping("/auth")
public class AuthenticationController {
    @GetMapping("/sayhello")
    public String sayHello(){
        return "Hello";
    }

    @GetMapping("/echo/{name}")
    public String echoName(@PathVariable String name) {
        if (name == null || name.isEmpty()) {
            return "Name is blank";
        }
        return "Hello " + name;
    }
}
```
Since we kept the controllers in ‘com.greekykhs.controller’ package, add the ComponentScan in Main class.


#### SpringSecurity21Application.java
```java
package com.prabhat.spring.security;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan("com.greekykhs.controller")
public class SpringSecurity21Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringSecurity21Application.class, args);
    }
}
```


## API Endpoints

Once the application is running, you can access the following GET REST web services directly:

- `http://localhost:8080/user/sayuser`
- `http://localhost:8080/user/echo/{message}`
- `http://localhost:8080/auth/sayhello`
- `http://localhost:8080/auth/echo/{message}`

## Adding Spring Security


To add Spring Security to the project, follow the steps below.

Now let’s say we don’t want anyone to directly access these services, instead we want our users to enter user name and password. For this we need to make a small modification in pom.xml and add ‘spring-boot-starter-security’ dependency.

1. Add the `spring-boot-starter-security` dependency to your `pom.xml`.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2. After adding this dependency and restarting the application, Spring Security will require a username and password to access the services. The default username is user, and you can 
   get the password from the console (generated by Spring Security).
3. To customize the username and password, add the following properties in application.properties:



Now, if you restart the spring boot project after adding this dependency and try to access any web-service you will get a prompt to enter user name and password ( form-based authentication), this is the default behavior for spring security.


The user name, by default is ‘user’ and we can get the password from the console for our application. When you check the console you will see a user-generated security password.

<img width="1378" alt="Screenshot 2024-12-31 at 6 00 28 PM" src="https://github.com/user-attachments/assets/52c220d2-777d-4273-a5fa-0f21ae35cd55" />

Let’s say you do not want to use autogenerated password and username as ‘user’, in that case we can configure it via application.properties.

`spring.security.user.name=Prabhat`

`spring.security.user.password=pass12345`

When you add above two lines in the application.properties and restart the application, you won’t see ‘generated security password’ on the console. And you can access the web-services with user name as ‘Prabhat’ and password as ‘pass12345’.




## Spring Security Workflow

The below diagram illustrates how Spring Security works internally.

![image](https://github.com/user-attachments/assets/af02282f-33af-4f95-85bf-af62a4f01ba2)

1. **User Credentials**: 
   When the user enters their credentials, an **authentication filter** in the Spring Security framework intercepts the request.

2. **Authentication Object**:
   The authentication filter then attempts to convert the authentication details received from the user into an **authentication object**. This object serves as the foundation for validating the user's credentials in subsequent steps.

3. **Authentication Manager**:
   The **Authentication Manager** determines which authentication provider to use. For example, this could be a database, OAuth, or LDAP to validate the user's details.

4. **Authentication Provider**:
   All the business logic related to security (e.g., how to validate the credentials) is contained within the **Authentication Provider**. This provider utilizes two key components:
   - **User Details Service**: Defines the user schema and how user details should look.
   - **Password Encoder**: Specifies how the password should be encoded or decrypted.

5. **Validation Process**:
   Once the **Authentication Provider** validates the user's credentials using the **User Details Service** and **Password Encoder**, the request is sent back to the **Authentication Manager**, followed by the **Authentication Filter**.

6. **Authentication Object Update**:
   The authentication object, which was initially sent from the **Authentication Filter**, will now hold the information about whether the user is valid, along with additional details such as authorities and roles.

7. **Security Context**:
   The **Authentication Filter** passes the authentication object to the **Security Context**, where it is stored in the Spring container. This authentication object is returned to the browser.

8. **Token Validation**:
   When the browser sends a request again, Spring Security will validate if the authentication object contains a valid token or not.

This flow ensures that the application is secured and the user's authentication details are validated properly.







### Join my Organization: 
Link: <a href="https://github.com/Tech-Hubs" target="_blank">PrabhatDevLab</a>


<p>Happy Learning! 📚✨ Keep exploring and growing your knowledge! 🚀😊</p>

