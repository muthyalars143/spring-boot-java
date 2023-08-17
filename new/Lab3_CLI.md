
## **Spring Boot CLI Lab: Creating a Web Application**

### **Objective:**
Learn how to create a simple web application using the Spring Boot CLI.

### **Prerequisites:**
- JDK 17
- Spring Boot CLI

---

### **Steps:**

1. **Installation of Spring Boot CLI:**
    - If you're using SDKMAN!, it's straightforward:
        ```bash
        sdk install springboot
        ```

    - For other installation methods, refer to [Spring Boot CLI Installation](https://docs.spring.io/spring-boot/docs/current/reference/html/cli-installation.html).

2. **Create a Simple Web Application:**

    Write a Groovy script named `app.groovy`:

    ```groovy
    @RestController
    class WebController {
        @RequestMapping("/")
        String home() {
            "Hello, Spring Boot CLI!"
        }
    }
    ```

3. **Run the Web Application:**

    Use the Spring Boot CLI to run the application:

    ```bash
    spring run app.groovy
    ```

    This will start a web server on port 8080. You can access the application at [http://localhost:8080](http://localhost:8080).

4. **Package the Web Application:**

    If you want to package your application into a standalone `.jar`:

    ```bash
    spring jar app.jar app.groovy
    ```

    Run the packaged application:

    ```bash
    java -jar app.jar
    ```

5. **Enhance the Web Application:**

    Integrate with Spring Boot Actuator to gain production-ready features:

    Modify `app.groovy` to add the actuator:

    ```groovy
    @Grab("spring-boot-starter-actuator")
    ```

    Restart the application and explore endpoints like `/actuator/health`.

6. **Cleanup:**

    Stop the application by pressing `CTRL+C` in the terminal.

---

### **Conclusion:**

You've learned how to use the Spring Boot CLI to create, run, and package a web application with ease. This tool is especially powerful for rapid prototyping and testing Spring Boot features.
