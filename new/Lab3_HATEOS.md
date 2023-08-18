
# **HATEOAS Driven Banking Services with Spring Boot**

## **Use Case: Savings Account Management**

In the age of digital banking, customers expect seamless experiences when interacting with financial platforms. HATEOAS provides a way to make these interactions more dynamic and intuitive by offering related operations directly in the response. This allows clients to navigate and discover available services without requiring prior knowledge of the entire API. In this lab, we will demonstrate this by implementing a "Savings Account Management" service using Spring Boot and HATEOAS.

## **Lab Setup in Gitpod**

### **1. Open Terminal in Gitpod**

Begin by launching the terminal from the Gitpod interface. This is where you'll execute all the commands for this lab.

### **2. Install JDK 17**

Java is the backbone of our application. JDK 17 is the latest long-term support version that brings several new features and improvements.

```bash
sdk install java 17.0.8-oracle
sdk use java 17.0.8-oracle
```


### **3. Create and Navigate to the Project Directory**

Organizing our workspace is crucial. We'll create a separate directory for our project to ensure that all our files are in one place.

```bash
mkdir -p /workspace/spring-boot-java/savings-account-service
```

### **4. Initialize Your Spring Boot Application with Gradle**

Spring Initializr is a convenient tool that bootstraps our project with necessary configurations. We'll use Gradle, a powerful build tool that automates the building, testing, publishing, and more.

```bash
curl https://start.spring.io/starter.zip -o savings-account-service.zip
unzip savings-account-service.zip -d savings-account-service
cd savings-account-service
```


#### **Main Application Class**

To create the main application class, navigate to the appropriate directory and use a text editor (like `nano` or `vim`) to create the file:

```bash
mkdir -p /workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount
touch /workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount/SavingsAccountApplication.java
```

#### **SavingsAccount Entity**

For the `SavingsAccount` entity:

```bash
touch /workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount/SavingsAccount.java
```

#### **SavingsAccountController**

For the `SavingsAccountController`:

```bash
touch /workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount/SavingsAccountController.java
```


#### **SavingsAccount Entity**

Create the entity class at `/workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount/SavingsAccount.java`.

#### **SavingsAccountController**

Create the controller class at `/workspace/spring-boot-java/savings-account-service/src/main/java/com/example/savingsaccount/SavingsAccountController.java`.


Organizing our workspace is crucial. We'll create a separate directory for our project to ensure that all our files are in one place.

```bash
mkdir -p /workspace/spring-boot-java/savings-account-service
cd /workspace/spring-boot-java/savings-account-service
```

### **4. Initialize Your Spring Boot Application with Gradle**

Spring Initializr is a convenient tool that bootstraps our project with necessary configurations. We'll use Gradle, a powerful build tool that automates the building, testing, publishing, and more.

```bash
curl https://start.spring.io/starter.zip -o savings-account-service.zip
unzip savings-account-service.zip -d savings-account-service
cd savings-account-service
```

### **5. Integrate Spring HATEOAS**

Spring HATEOAS offers APIs to create web links. This is central to the concept of HATEOAS where resources guide clients through actions via links.

Edit the `build.gradle` file to include the Spring HATEOAS dependency:

```groovy
implementation 'org.springframework.boot:spring-boot-starter-hateoas'
```

### **6. Implement Savings Account API with HATEOAS**

Now, we'll dive into the core of our application.

#### **SavingsAccount Entity**

The SavingsAccount entity represents a bank savings account. It will contain attributes like an ID, account number, and balance:

```java
@Entity
public class SavingsAccount {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String accountNumber;
    private Double balance;

    // Default constructor
    public SavingsAccount() {}

    // Parameterized constructor
    public SavingsAccount(String accountNumber, Double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }

    // hashCode, equals, and toString methods
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        SavingsAccount that = (SavingsAccount) o;
        return id.equals(that.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    @Override
    public String toString() {
        return "SavingsAccount{" +
               "id=" + id +
               ", accountNumber='" + accountNumber + ''' +
               ", balance=" + balance +
               '}';
    }
}

    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String accountNumber;
    private Double balance;

    public SavingsAccount() {}

    public SavingsAccount(String accountNumber, Double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        SavingsAccount that = (SavingsAccount) o;
        return Objects.equals(id, that.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    @Override
    public String toString() {
        return "SavingsAccount{" +
                "id=" + id +
                ", accountNumber='" + accountNumber + ''' +
                ", balance=" + balance +
                '}';
    }
}

    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String accountNumber;
    private Double balance;

    public SavingsAccount() {
    }

    public SavingsAccount(String accountNumber, Double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    // Standard getters and setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }
}

```


#### **SavingsAccountController**

Controllers in Spring Boot handle incoming web requests. By enriching responses with HATEOAS links, we can guide clients on possible interactions:

```java

@RestController
@RequestMapping("/savings-accounts")
public class SavingsAccountController {

    @Autowired
    private SavingsAccountRepository repository;  // Assume a JpaRepository for SavingsAccount

    @GetMapping("/{id}")
    public ResponseEntity<EntityModel<SavingsAccount>> retrieveAccount(@PathVariable Long id) {
        Optional<SavingsAccount> accountOpt = repository.findById(id);
        
        if (!accountOpt.isPresent()) {
            return ResponseEntity.notFound().build();
        }
        
        SavingsAccount account = accountOpt.get();
        
        EntityModel<SavingsAccount> resource = EntityModel.of(account);
        WebMvcLinkBuilder linkTo = WebMvcLinkBuilder.linkTo(WebMvcLinkBuilder.methodOn(this.getClass()).retrieveAccount(id));
        
        resource.add(linkTo.withRel("self"));
        
        return ResponseEntity.ok(resource);
    }
}

```

#### **Main Application Class**

Every Spring Boot application requires a main class to serve as its entry point. This class is responsible for setting up and launching our application. 

Create a class named `SavingsAccountApplication.java`:

```java
package com.example.savingsaccount;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SavingsAccountApplication {

    public static void main(String[] args) {
        SpringApplication.run(SavingsAccountApplication.class, args);
    }
}
```

In this class:

- `@SpringBootApplication` is a convenience annotation that adds several other annotations like `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. This effectively sets up the configuration and scanning of Spring components.
- The `main` method contains `SpringApplication.run()`, which launches the Spring Boot application.

With this main class in place, you can run the application using the provided Gradle command in step 7.



### **7. Testing the HATEOAS Driven Service**

#### **Starting the Application**
Run the Spring Boot application using Gradle:

```bash
./gradlew bootRun
```

Once the application is running, you can test the following scenarios:

#### **1. Fetching a Savings Account**
Visit `http://localhost:8080/savings-accounts/1` in your browser. The response should display details of the first savings account, enriched with a self-referential HATEOAS link.

#### **2. Handling Non-existent Accounts**
Try accessing an account that doesn't exist, for example, `http://localhost:8080/savings-accounts/10`. This should return a 404 Not Found status, indicating the account does not exist.

#### **3. Exploring HATEOAS Links**
In the response of the first scenario, click on the provided HATEOAS links. They should guide you to other related functionalities or details of the API.

#### **4. Testing Invalid Input**
Try accessing the API with invalid input, like a non-numeric ID: `http://localhost:8080/savings-accounts/abc`. This should also return an error message, highlighting the importance of input validation in RESTful services.

#### **5. Verifying Response Headers**
Using tools like `curl` or Postman, send a request to fetch an account and inspect the response headers. They should contain information like content type indicating it's a JSON response.

To test using `curl`:

```bash
curl -i http://localhost:8080/savings-accounts/1
```

Make sure to explore other features of the application and think of other edge cases to test, ensuring the robustness of the service.


Run the Spring Boot application using Gradle:

```bash
./gradlew bootRun
```

With the application running, test the various endpoints:

1. **Fetching a Savings Account**: Visit `http://localhost:8080/savings-accounts/1` in your browser. The response should display details of the first savings account, enriched with a self-referential HATEOAS link.

2. **Handling Non-existent Accounts**: Try accessing an account that doesn't exist, for example, `http://localhost:8080/savings-accounts/10`. This should return a 404 Not Found status, indicating the account does not exist.

Ensure to explore the provided HATEOAS links in the responses to understand the dynamic nature of the API.


With everything set up, it's time to see our HATEOAS service in action.

```bash
./gradlew bootRun
```

Visit `http://localhost:8080/savings-accounts/1` in your browser. The response should contain account details alongside HATEOAS links guiding further actions.

## **Bonus Challenge**

Reinforce your learning by applying the principles:

1. Think of a new banking use case, perhaps "Loan Management".
2. Design entities and operations pertinent to this use case.
3. Implement and enrich responses with HATEOAS links.
4. Share with peers, gather feedback, and iterate.
