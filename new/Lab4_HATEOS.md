
## **Spring Boot Microservices Lab: Implementing HATEOAS**

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

    Create an `Employee` entity with fields such as `id`, `name`, and `role`.

    ```java
    @Entity
    public class Employee {
        @Id @GeneratedValue
        private Long id;
        private String name;
        private String role;
        
        // Getters, setters, and other standard methods
    }
    ```

4. **Create a Repository:**

    Create an `EmployeeRepository` interface extending `JpaRepository`:

    ```java
    public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    }
    ```

5. **Implement HATEOAS in Controller:**

    Create a REST controller for the `Employee` entity:

    ```java
    @RestController
    public class EmployeeController {
        
        @Autowired
        private EmployeeRepository repository;
        
        @GetMapping("/employees")
        public CollectionModel<EntityModel<Employee>> all() {
            List<EntityModel<Employee>> employees = repository.findAll().stream()
                .map(employee -> EntityModel.of(employee, 
                    linkTo(methodOn(EmployeeController.class).one(employee.getId())).withSelfRel(),
                    linkTo(methodOn(EmployeeController.class).all()).withRel("employees")))
                .collect(Collectors.toList());
            
            return CollectionModel.of(employees, linkTo(methodOn(EmployeeController.class).all()).withSelfRel());
        }
        
        @GetMapping("/employees/{id}")
        public EntityModel<Employee> one(@PathVariable Long id) {
            Employee employee = repository.findById(id).orElseThrow(() -> new RuntimeException("Not found"));

            return EntityModel.of(employee,
                linkTo(methodOn(EmployeeController.class).one(id)).withSelfRel(),
                linkTo(methodOn(EmployeeController.class).all()).withRel("employees"));
        }
    }
    ```

6. **Testing the Application:**

    Run the application:

    ```bash
    ./gradlew bootRun
    ```

    Once the application is running, you can use a web browser or tools like `curl` to access the endpoints:

    ```bash
    curl http://localhost:8080/employees
    ```

    The HATEOAS links should guide you to related resources.

7. **Cleanup:**

    Stop the application by pressing `CTRL+C` in the terminal.

---

### **Conclusion:**

You've successfully implemented a Spring Boot microservice that uses HATEOAS principles. The hypermedia links provided by the API make it self-discovering, guiding the consumers to relevant resources and possible actions.
