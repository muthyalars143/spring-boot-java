
## **Spring Boot Microservices Lab: Implementing HATEOAS**

### **Introduction to HATEOAS:**

HATEOAS, or Hypermedia as the Engine of Application State, is a principle of the REST architectural style. It means that the API provides information dynamically through hypermedia, allowing clients to interact with its resources without prior knowledge of its structure. In simpler terms, a client communicates with a HATEOAS-driven API by navigating hypermedia provided dynamically by the server. This approach makes APIs more self-descriptive and adaptive to changes.

### **Objective:**
Create a Spring Boot microservice that demonstrates the principles of HATEOAS.

### **Prerequisites:**
- JDK 17
- Gradle
- An IDE or text editor

---

### **Steps:**

1. **Setting Up the Environment:**

    - Install JDK 17 if not already done. The installation process varies based on the operating system.
    - Install Gradle. You can use package managers like SDKMAN!, Homebrew (for macOS), or apt (for Ubuntu) to do this.

2. **Bootstrap Your Spring Boot Application:**

    Use the Spring Initializer to generate a Gradle project:

    ```bash
    curl https://start.spring.io/starter.zip -d dependencies=web,hateoas,data-jpa -d type=gradle-project -d javaVersion=17 -o hateoas-demo.zip
    unzip hateoas-demo.zip
    cd hateoas-demo
    ```

3. **Define the Domain Model:**

    Navigate to `src/main/java/com/example/demo/domain` and create an `Employee` entity with fields such as `id`, `name`, and `role`.

    ```java
    package com.example.demo.domain;

    import javax.persistence.Entity;
    import javax.persistence.GeneratedValue;
    import javax.persistence.Id;

    @Entity
    public class Employee {
        @Id 
        @GeneratedValue
        private Long id;
        private String name;
        private String role;
        
        // Getters, setters, and other standard methods
    }
    ```

    Explanation:
    - `@Entity`: This annotation marks the class as a JPA entity. This means that it will be mapped to a database table.
    - `@Id`: This annotation marks the `id` field as the primary key.
    - `@GeneratedValue`: This annotation indicates that the `id` value will be automatically generated.

4. **Create a Repository:**

    Navigate to `src/main/java/com/example/demo/repository` and create an `EmployeeRepository` interface extending `JpaRepository`.

    ```java
    package com.example.demo.repository;

    import com.example.demo.domain.Employee;
    import org.springframework.data.jpa.repository.JpaRepository;

    public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    }
    ```

    Explanation:
    - The interface extends `JpaRepository`, which provides basic CRUD operations without needing to write any implementation.

5. **Implement HATEOAS in Controller:**

    Navigate to `src/main/java/com/example/demo/controller` and create a REST controller for the `Employee` entity.

    [Note: The rest of the content will be processed next.]


    ```java
    package com.example.demo.controller;

    import com.example.demo.domain.Employee;
    import com.example.demo.repository.EmployeeRepository;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.hateoas.CollectionModel;
    import org.springframework.hateoas.EntityModel;
    import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    import java.util.List;
    import java.util.stream.Collectors;

    @RestController
    @RequestMapping("/employees")
    public class EmployeeController {
        
        @Autowired
        private EmployeeRepository repository;
        
        @GetMapping("/")
        public CollectionModel<EntityModel<Employee>> all() {
            List<EntityModel<Employee>> employees = repository.findAll().stream()
                .map(employee -> EntityModel.of(employee, 
                    WebMvcLinkBuilder.linkTo(WebMvcLinkBuilder.methodOn(EmployeeController.class).one(employee.getId())).withSelfRel()))
                .collect(Collectors.toList());

            return CollectionModel.of(employees, WebMvcLinkBuilder.linkTo(WebMvcLinkBuilder.methodOn(EmployeeController.class).all()).withSelfRel());
        }

        @GetMapping("/{id}")
        public EntityModel<Employee> one(Long id) {
            Employee employee = repository.findById(id).orElseThrow(() -> new RuntimeException("Employee not found"));
            return EntityModel.of(employee, WebMvcLinkBuilder.linkTo(WebMvcLinkBuilder.methodOn(EmployeeController.class).one(id)).withSelfRel());
        }
    }
    ```

    Explanation:
    - `@RestController`: This annotation indicates that the class will serve as a REST controller. 
    - `@RequestMapping("/employees")`: This sets the base URL for this controller.
    - `@Autowired`: This annotation allows Spring to automatically inject the `EmployeeRepository` instance.
    - The `all()` method retrieves all employees and enriches them with HATEOAS links.
    - The `one(Long id)` method retrieves a single employee by ID and adds HATEOAS links.
    - `WebMvcLinkBuilder.linkTo()`: This method constructs a link to a method in the controller, allowing us to build the hypermedia links dynamically based on the method names.

6. **Run Your Application:**

    Navigate to the root of your project (`hateoas-demo`) and run:

    ```bash
    ./gradlew bootRun
    ```

    Once the application is running, you can access `http://localhost:8080/employees` to see the list of employees with HATEOAS links.

---

That concludes the lab on implementing HATEOAS in a Spring Boot microservice. By following these steps, you've successfully set up a microservice that adheres to the principles of HATEOAS, making it more self-descriptive and adaptable to changes.
