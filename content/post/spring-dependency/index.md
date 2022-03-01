# API Design using Spring Dependency Injection

Dependency injection in Spring utilizes Beans (objects that are instantiated at boot time) and concept known as Autowiring. It is known as Autowirng beacause the Spring Framework 
does this process automatically. Dependency injection helps in seperation of the back-end's functions so that it has an API layer, Service layer, and a Data Access layer by decoupling (or loose coupling) the various components. The objects(beans) thus have to be injected at boot time.

In this article, we will use constructor based injection for demonstration. The example class will be a `student` class with the following properties:
- Id `id`,
- Name `name`,
- email `email`,
- Date of Birth `dob` and 
- Age `age`.

# Table of Contents

[Initializing a Spring Project](#initializing-spring-using-spring-initializr)

[Creating a Controller Class](#creating-a-student-controller-class)

[Creating a Service Class](#creating-a-student-service-class)

[Illustrating Dependency injection](#illustration-of-the-dependency-process)

[References](#references)

# Initializing Spring using Spring Initializr
Go to https://start.spring.io/ and select the parameters in the following snippet or use [this link](https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.6.3&packaging=jar&jvmVersion=17&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot%20Dep%20Injection&packageName=com.example.dependency&dependencies=web) for the populated fields.

`project:maven
spring boot: 2.6.3
package name: com.example.dependency
packaging: jar
java: 17
dependencies: Spring Web
`

Generate (CTRL + ENTER) the project files and unzip the folder then open the `/demo/` folder using an IDE of your choice.



Create a Java Package/Folder called `student` inside the `/src/main/java/com/example/dependency/` folder to contain the  demonstration Student class.

Inside it, create the Student class, `Student.java` and paste the code below:
```java
package com.example.demo.student;

import java.time.LocalDate;

public class Student {
    
    private Long id;
    private String name;
    private String email;
    private LocalDate dob;
    private Integer age;

    public Student(){

    }

    public Student(Long id,String name,String email,LocalDate dob, Integer age){
        this.id=id;
        this.name=name;
        this.email=email;
        this.age=age;
        this.dob=dob;
    }

    public Student(String name,String email,LocalDate dob, Integer age){
        this.name=name;
        this.email=email;
        this.age=age;
        this.dob=dob;
    }

    public Long getId(){
        return id;
    }

    public void setId(){
        this.id=id;
    }

    public String getName(){
        return name;
    }

    public void setName(){
        this.name=name;
    }

    public String getEmail(){
        return email;
    }

    public void setEmail(){
        this.email=email;
    }

    public Integer getAge(){
        return age;
    }

    public void setAge(){
        this.age=age;
    }

    public LocalDate getDob(){
        return dob;
    }

    public void setDob(){
        this.dob=dob;
    }

    @Override
    public String toString(){
        return "Student{"+
                "id="+id+
                ", name='"+name+'\''+
                ", email='"+email+'\''+
                ", dob='"+dob+
                ", age='"+age+
                "}";
    }
}

```


# Creating a Student Service class

In this the `/student/` folder, create a class `StudentService` (a `StudentService.java` file). This will act as our Service layer's handler.

Inside this class we will create a simple Spring Bean component that returns a simple JSON response of a student `John Doe`. Remember to decorate the class using the `@Component` decorator. You can achieve the same by pasting the code in the following cell:

```java
package com.example.demo.student;


import org.springframework.stereotype.Component;

import java.util.List;

import java.time.LocalDate;
import java.time.Month;

@Component
public class StudentService {
    public List<Student> getStudents(){
		return List.of(
			new Student(
				1L,
				"John Doe",
				"john.doe@gmail.com",
				LocalDate.of(2000,Month.APRIL,4),
				21
			)
		);
	}
}

```

# Creating a Student Controller class

In this the `/student/` folder, create a class `StudentController` (a `StudentController.java` file). This will act as our API layer's controller.

The API layer handles requests using the `@RequestMapping` annotation with its path parameters and calls classes and objects from the Service layer (which handles business logic).

Inside this class , by pasting the code in the following cell:

```java
package com.example.demo.student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


import java.util.List;



@RestController
@RequestMapping(path="api/student")

public class StudentController {
    
	private final StudentService studentService;
  
    /** The studentService object is instantiated and injected here (autowiring) **/
	@Autowired
	public StudentController(StudentService studentService){
		this.studentService=studentService;
	}
    @GetMapping
	public List<Student> getStudents(){
		return studentService.getStudents();
	}
}

```

# Illustration of the Dependency process
In our example, the `studentService` object is instantiated  and injected into the `StudentController` class when the controller is booted.

Run `DemoApplication.java` in the `demo/src/main/java/com/example/dependency/` folder and look at the resulting JSON response on port 8080 (http://localhost:8080/)

# References
[Dependency Injection In Spring](https://www.educba.com/dependency-injection-in-spring/)

[Spring Framework Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)

