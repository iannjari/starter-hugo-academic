---
title: Springboot Data Transfer Objects (DTOs)

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
    
    ...
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

# Creating a DTO Package and Class



# Convert JPA Entity to DTO


# Call a Mapped DTO from the Controller Layer


# References


