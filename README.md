# Spring Security


Authentication v/s Authorization


Authentication and Authorization are often used in conjunction with each other in terms of security, especially when it comes to gaining access to the system.

Authentication means confirming your own identity, while authorization means granting access to the system. We can say, authentication is the process of verifying who you are, while authorization is the process of verifying what you have access to.


I see! Here's the same content with just the alignment fixed, as you originally requested:

Authentication :

* While doing the authentication, we validate the credentials like user name or user id and password.

* Authentication factors determine the various elements the system uses to verify one’s identity prior to granting him access to anything from accessing a file to requesting a bank transaction.

* A user’s identity can be determined by what he knows, what he has, or what he is.


When it comes to security, at least two or all the three authentication factors must be verified in order to grant someone access to the system.


* Single-Factor Authentication: It commonly relies on a simple password to grant user access to a particular system such as a website or a network.
* Two-Factor Authentication: It’s a two-step verification process which not only requires a username and password, but also something only the user knows, to ensure an additional level of security, such as an ATM pin or security answers, which only the user knows.

* Multi-Factor Authentication: This is the most advanced method of authentication which uses two or more levels of security from independent categories of authentication to grant user access to the system. All the factors should be independent of each other to eliminate any vulnerability in the system.
—————————————————————————————————————————————————————————————  



Authorization :

* Authorization occurs after the identity is successfully authenticated by the system, which ultimately gives you full permission to access the resources such as information, files, databases, funds, locations, almost anything.

* Authorization is the process to determine whether the authenticated user has access to the particular resources.

* It verifies your rights to grant you access to resources.

* Authorization usually comes after authentication which confirms your privileges to perform.
—————————————————————————————————————————————————————————————  



What is the difference between Authentication and Authorization?

* Authentication confirms your identity to grant access to the system. Authorization determines whether you are authorized to access the resources.

* Authentication is the process of validating user credentials to gain user access. Authorization is the process of verifying whether access is allowed or not.
* Authentication determines whether the user is what he claims to be. Authorization determines what users can and cannot access.

* Authentication usually requires a username and a password. Authentication factors required for authorization may vary, depending on the security level.
* Authorization is done after successful authentication.
—————————————————————————————————————————————————————————————  





Let’s create a Spring Starter Project from STS, as of now this project has only Spring Web dependency.

￼

Create controller classes AuthenticationController.java and UserController.java inside it.


￼
     
                                                        👆 UserController.java 👆



￼

                                                    👆 AuthenticationController.java 👆



Since we kept the controllers in ‘com.greekykhs.controller’ package, add the ComponentScan in Main class.


￼

                                                              👆 SpringSecurity21Application.java 👆





Once we run our Spring boot project, we can access below get rest webservices directly.
* 		http://localhost:8080/user/sayuser
* 		http://localhost:8080/user/echo/Hello!!
* 		http://localhost:8080/auth/sayhello
* 		http://localhost:8080/auth/echo/Michael

GIT URL: spring-security-21



Now let’s say we don’t want anyone to directly access these services, instead we want our users to enter user name and password. For this we need to make a small modification in pom.xml and add ‘spring-boot-starter-security’ dependency.


<dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-security</artifactId> </dependency>



Now, if you restart the spring boot project after adding this dependency and try to access any web-service you will get a prompt to enter user name and password ( form-based authentication), this is the default behavior for spring security.

The user name, by default is ‘user’ and we can get the password from the console for our application. When you check the console you will see a user-generated security password.

￼

Let’s say you do not want to use autogenerated password and username as ‘user’, in that case we can configure it via application.properties.

spring.security.user.name=Prabhat spring.security.user.password=pass12345

When you add above two lines in the application.properties and restart the application, you won’t see ‘generated security password’ on the console. And you can access the web-services with user name as ‘greekykhs’ and password as ‘pass12345’.






￼
                 
                                                                                      Spring Security Flow


- [x] Above diagram show, how Spring Security works internally.

- [x] When the user enters the credentials, an authentication filter present in spring security framework intercepts the request.

- [x] After that it will try to convert the authentication details received from the user into an authentication object. This object is the base, where all the validation of user credentials will be validated in further steps.

- [x] Authentication Manager will identify what is the authentication provider that the request has to go. e.g we may use the database, oauth or ldap to validate the details.

- [x] All the business related logics (related to security e.g how to validate the details?) will be present in Authentication Provider. It in turn uses two other interfaces User Details Service and Password Encoder.

- [x] A User Details Service holds the user schema, like how my user details should look like. Password Encoder will tell how the password has to be encoded/ decrypted.

- [x] Once Authentication Provider validates the input using User Details Service and Password Encoder, it will be transfer the request to Authentication Manager, followed by Authentication Filter.

- [x] Now the authentication object, which we had initially send from the Authentication Filter will hold the information whether the user is valid or not, along with other details like authorities, roles etc.

- [x] The authentication filter will pass the authentication object to Security Context, where the details will be stored in the (Spring) container. This authentication object is given back to the browser, when the browser wants to send the request second time it Spring security will validate if the authentication object has valid token or not.


