---
title: Using Springboot Data Transfer Objects (DTOs) to hide sensitive data

# Date published
date: "2022-03-10T00:00:00Z"

# Date updated
lastmod: "2022-03-10T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: true

authors:
- admin

tags:
- Spring
- DTO
- API

---

Spring DTOs help API developers serve data from JPA entities without exposing high risk data like passwords, or serving bulky unnecessary data to the client.
DTos can be used to perform validation of data between the JPA repository and the Service Layer and client.

For example, if you have a User model that has a password among other fields as shown below, you might not want to serve the password to a REST client.

```java
@Entity
 @Table
public class AppUser {
    @Id
     @SequenceGenerator(
            name = "users_sequence",
            sequenceName = "users_sequence",
            allocationSize = 1
    )

    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator="users_sequence"
    )
    private Long id;
    private String username;
    private String name;
    private String email;
    private String photo;
    private String password;
    
 }
```

Let's explore how to serve the other fields without the passwords.

> *Note* : It is assumed that you have knowledge of building Spring RESTful applications.


# Table of Contents

[How Spring DTOs work](#how-spring-dtos-work)

[Creating a DTO Package and Class](#creating-a-dto-package-and-class)

[Convert JPA Entity to DTO](#convert-jpa-entity-to-dto)

[Call a Mapped DTO from the Controller Layer](#call-a-mapped-dto-from-the-controller-layer)

[References](#references)


# How Spring DTOs work

DTos involve mapping of a queried JPA object into a format where the fields have been modified or ommitted. A query is made to the database through a repository, the query function from the Service layer is made to return a mapped object. The mapping function is declared in the Service layer while the DTO class, which defines objects to be published to the client, is defined inside it's own package or as inside the model (not recommended).

Inside the controller class, the response body is made to return an object obtained through the invocation of the query function, where the mapping happened.

# Creating a DTO Package and Class

In this demonstration, we will create a DTO to mask the password field in the AppUser class defined by the model above.

In an already initialised Spring project, create a DTO package inside the package holding the `AppUser` model. Inside it, create a Java class `UserDto` with the following model:

```java
package app.ultratechies.api.AppUsers.UserDTO;

public class UserDto {
        Long id;
        String username;
        String name;
        String email;
        String photo;
// Include getter and setter methods for the class using lombok or manually
```

Note that the field `password` is not included in the class since we don't want to map it in the responses.

# Convert JPA Entity to DTO
We first need to query the database using a `getById` Repository query in the User Service class as shown below;

```java
public Optional<AppUser> getUsersById(Long id) {
        boolean exists= userRepository.existsById(id);
        if (!exists){
            throw new IllegalStateException("user with userId:"+ id +" does not exist!");
        }
        return (Optional<AppUser>) userRepository.findById(id);
    }
```

Note that the `getUsersById` function returns an object from the `AppUser` class as expected.
To return a `UserDto` class object, we need to change the return classes to `UserDto` as shown;

```java
public Optional<UserDto> getUsersById(Long id) {
        boolean exists= userRepository.existsById(id);
        if (!exists){
            throw new IllegalStateException("user with userId:"+ id +" does not exist!");
        }
        return (Optional<UserDto>) userRepository.findById(id);
    }
```
We then add a mapping method to the `findById` query at the return statement like;
```java
return (Optional<UserDto>) userRepository.findById(id).map(this::convertUserDto);
```
You may note that the `convertUserDto` function cannot be resolved. This is because we have not declared it yet.
Immediately below, define it as follows;
```java
private UserDto convertUserDto(AppUser appUser){
        UserDto userdto = new UserDto();
        userdto.setId(appUser.getId());
        userdto.setUsername(appUser.getUsername());
        userdto.setName(appUser.getName());
        userdto.setEmail(appUser.getEmail());
        userdto.setPhoto(appUser.getPhoto());

        return userdto;
    }
```
This function receives an `AppUser` object `appUser`, creates an empty `UserDto` object `userdto` and extracts the necessary fields from the `appUser`, (a JPA entity of the `Appuser` class) and sets them to the `userdto` object and returns it. therefore, the return object of the `getUsersById` function is now a `userdto` object.

# Call a Mapped DTO from the Controller Layer



# References


